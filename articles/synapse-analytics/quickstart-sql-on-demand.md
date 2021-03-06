---
title: 使用 SQL 隨選 (預覽)
description: 在本快速入門中，您將了解如何使用 SQL 隨選 (預覽) 來查詢各種類型的檔案。
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 43f361fbaf4ab0462af0a720d7711f219134a165
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82692161"
---
# <a name="quickstart-using-sql-on-demand"></a>快速入門：使用 SQL 隨選

Synapse SQL 隨選 (預覽) 是一項無伺服器查詢服務，可讓您對放在 Azure 儲存體中的檔案執行 SQL 查詢。 在本快速入門中，您將了解如何使用 SQL 隨選來查詢各種類型的檔案。

以下是支援的檔案類型：JSON、CSV、Apache Parquet

## <a name="prerequisites"></a>Prerequisites

選擇要發出查詢的 SQL 用戶端：

- [Azure Synapse Studio](quickstart-synapse-studio.md) 是一種 Web 工具，可用來瀏覽儲存體中的檔案並建立 SQL 查詢。
- [Azure Data Studio](sql/get-started-azure-data-studio.md) 是一種用戶端工具，可讓您在隨選資料庫上執行 SQL 查詢和筆記本。
- [SQL Server Management Studio](sql/get-started-ssms.md) 是一種用戶端工具，可讓您在隨選資料庫上執行 SQL 查詢。

用於快速入門的參數：

| 參數                                 | 描述                                                   |
| ----------------------------------------- | ------------------------------------------------------------- |
| SQL 隨選服務端點位址    | 作為伺服器名稱                                   |
| SQL 隨選服務端點區域     | 用來決定要在範例中使用的儲存體 |
| 用來存取端點的使用者名稱和密碼 | 用來存取端點                               |
| 用來建立檢視的資料庫         | 範例中用來作為起點的資料庫       |

## <a name="first-time-setup"></a>第一次設定

使用範例之前：

- 為您的檢視建立資料庫 (如果您想要使用檢視的話)
- 建立可讓 SQL 隨選用來存取儲存體中檔案的認證

### <a name="create-database"></a>建立資料庫

建立您自己的資料庫以供示範之用。 您將使用此資料庫來建立您的檢視和進行本文中的範例查詢。

> [!NOTE]
> 資料庫僅用於檢視的中繼資料，不適用於實際資料。
>記下您使用的資料庫名稱，以便稍後在快速入門中使用。

使用下列查詢，將 `mydbname` 變更為您選擇的名稱：

```sql
CREATE DATABASE mydbname
```

### <a name="create-credentials"></a>建立認證

若要使用 SQL 隨選執行查詢，請建立讓 SQL 隨選用來存取儲存體中檔案的認證。

> [!NOTE]
> 若要在本節中成功執行範例，您必須使用 SAS 權杖。
>
> 若要開始使用 SAS 權杖，您必須卸除 UserIdentity，如下列[文章](sql/develop-storage-files-storage-access-control.md#disable-forcing-azure-ad-pass-through)所述。
>
> SQL 隨選一律預設使用 AAD 傳遞。

如需有關如何管理儲存體存取控制的詳細資訊，請參閱[控制 SQL 隨選的儲存體帳戶存取](sql/develop-storage-files-storage-access-control.md)一文。

執行下列程式碼片段，以建立本節範例所使用的認證：

```sql
-- create credentials for containers in our demo storage account
IF EXISTS
   (SELECT * FROM sys.credentials
   WHERE name = 'https://sqlondemandstorage.blob.core.windows.net')
   DROP CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net]
GO

CREATE CREDENTIAL [https://sqlondemandstorage.blob.core.windows.net]
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = 'sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D'
GO
```

## <a name="querying-csv-files"></a>查詢 CSV 檔案

下圖是要查詢的檔案預覽：

![CSV 檔案中的前 10 個資料列沒有標頭，採用 Windows 樣式新行。](./sql/media/query-single-csv-file/population.png)

下列查詢顯示如何讀取不包含標頭資料列、採用 Windows 樣式新行及逗號分隔資料行的 CSV 檔案：

```sql
SELECT TOP 10 *
FROM OPENROWSET
  (
      BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population/*.csv'
    , FORMAT = 'CSV'
  )
WITH
  (
      country_code VARCHAR (5)
    , country_name VARCHAR (100)
    , year smallint
    , population bigint
  ) AS r
WHERE
  country_name = 'Luxembourg' AND year = 2017
```

您可以在查詢編譯時間上指定架構。
如需更多範例，請參閱如何[查詢 CSV 檔案](sql/query-single-csv-file.md)。

## <a name="querying-parquet-files"></a>查詢 Parquet 檔案

下列範例顯示用於查詢 Parquet 檔案的自動結構描述推斷功能。 其會傳回 2017 年 9 月的資料列數目，而不需要指定架構。

> [!NOTE]
> 在讀取 Parquet 檔案時，您不需要在 `OPENROWSET WITH` 子句中指定資料行。 在此案例中，SQL 隨選會利用 Parquet 檔案中的中繼資料，並依名稱繫結資料行。

```sql
SELECT COUNT_BIG(*)
FROM OPENROWSET
  (
      BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2017/month=9/*.parquet'
    , FORMAT='PARQUET'
  ) AS nyc
```

尋找更多有關[查詢 Parquet 檔案](sql/query-parquet-files.md)的資訊。

## <a name="querying-json-files"></a>查詢 JSON 資料

### <a name="json-sample-file"></a>JSON 範例檔案

檔案會儲存在 json  容器的 books 資料夾  中，並包含採用下列結構的單一書籍項目：

```json
{  
   "_id":"ahokw88",
   "type":"Book",
   "title":"The AWK Programming Language",
   "year":"1988",
   "publisher":"Addison-Wesley",
   "authors":[  
      "Alfred V. Aho",
      "Brian W. Kernighan",
      "Peter J. Weinberger"
   ],
   "source":"DBLP"
}
```

### <a name="querying-json-files"></a>查詢 JSON 資料

下列查詢會示範如何使用 [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)，從名為 *Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected articles* 的書籍中擷取純量值 (書名、發行者)：

```sql
SELECT
    JSON_VALUE(jsonContent, '$.title') AS title
  , JSON_VALUE(jsonContent, '$.publisher') as publisher
  , jsonContent
FROM OPENROWSET
  (
      BULK 'https://sqlondemandstorage.blob.core.windows.net/json/books/*.json'
    , FORMAT='CSV'
    , FIELDTERMINATOR ='0x0b'
    , FIELDQUOTE = '0x0b'
    , ROWTERMINATOR = '0x0b'
  )
WITH
  ( jsonContent varchar(8000) ) AS [r]
WHERE
  JSON_VALUE(jsonContent, '$.title') = 'Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics'
```

> [!IMPORTANT]
> 我們會將整個 JSON 檔案讀取為單一資料列/資料行。 因此，FIELDTERMINATOR、FIELDQUOTE 和 ROWTERMINATOR 會設定為 0x0b，因為我們不會在檔案中尋找此項目。

## <a name="next-steps"></a>後續步驟

您現在已準備好繼續進行下列文章：

- [查詢單一 CSV 檔案](sql/query-single-csv-file.md)
- [查詢資料夾和多個 CSV 檔案](sql/query-folders-multiple-csv-files.md)
- [查詢特定檔案](sql/query-specific-files.md)
- [查詢 Parquet 檔案](sql/query-parquet-files.md)
- [查詢 Parquet 巢狀型別](sql/query-parquet-nested-types.md)
- [查詢 JSON 檔案](sql/query-json-files.md)
- [建立和使用檢視](sql/create-use-views.md)
- [建立和使用外部資料表](sql/create-use-external-tables.md)
- [將查詢結果保存到 Azure 儲存體](sql/create-external-table-as-select.md)
- [查詢單一 CSV 檔案](sql/query-single-csv-file.md)
