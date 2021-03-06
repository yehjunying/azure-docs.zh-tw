---
title: Azure Synapse SQL 中的 T-SQL 功能差異
description: Synapse SQL 中可用的 Transact-SQL 功能清單。
services: synapse analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jovanpop
ms.reviewer: jrasnick
ms.openlocfilehash: b3e4d705bea7243966959038cf15373097c73ebc
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81421282"
---
# <a name="transact-sql-features-supported-in-azure-synapse-sql"></a>Azure Synapse SQL 中支援的 Transact-SQL 功能

Azure Synapse SQL 是一種巨量資料分析服務，可讓您使用 T-SQL 語言來查詢和分析資料。 您可以使用在 SQL Server 和 Azure SQL Database 上用來分析資料、符合 ANSI 規範的標準 SQL 語言方言。 

Transact-SQL 語言可用於 Synapse SQL 的無伺服器模型和已佈建模型中，兩者會參考不同物件，且所支援的功能集會有一些差異。 在此頁面中，您可以找到 Synapse SQL 耗用量模型之間的高階 Transact-SQL 語言差異。

## <a name="database-objects"></a>資料庫物件

Synapse SQL 中的耗用量模型可讓您使用不同的資料庫物件。 下表顯示所支援物件類型的比較：

|   | 已佈建 | 無伺服器 |
| --- | --- | --- |
| 資料表 | [是](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | 否，無伺服器模型只能查詢放在 [Azure 儲存體](#storage-options)上的外部資料 |
| 檢視 | [是](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)。 檢視可以使用已佈建模型中可用的[查詢語言元素](#query-language)。 | [是](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)。 檢視可以使用無伺服器模型中可用的[查詢語言元素](#query-language)。 |
| 結構描述 | [是](/sql/t-sql/statements/create-schema-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | [是](/sql/t-sql/statements/create-schema-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| 暫存資料表 | [是](../sql-data-warehouse/sql-data-warehouse-tables-temporary.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) | 否 |
| 程序 | [是](/sql/t-sql/statements/create-procedure-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | 否 |
| 函式 | [是](/sql/t-sql/statements/create-function-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | 否 |
| 觸發程序 | 否 | 否 |
| 外部資料表 | [是](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)。 請參閱支援的[資料格式](#data-formats)。 | [是](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)。 請參閱支援的[資料格式](#data-formats)。 |
| 快取查詢 | 是，多個表單 (SSD 型快取、記憶體內部、結果集快取)。 此外，也支援具體化檢視 | 否 |
| 資料表變數 | [否](/sql/t-sql/data-types/table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)，使用暫存資料表 | 否 |

## <a name="query-language"></a>查詢語言

Synapse SQL 中使用的查詢語言會根據耗用量模型而支援不同的功能。 下表概述 Transact-SQL 方言中最重要的查詢語言差異：

|   | 已佈建 | 無伺服器 |
| --- | --- | --- |
| SELECT 陳述式 | 是。 不支援 Transact-SQL 查詢子句 [FOR XML/FOR JSON](/sql/t-sql/queries/select-for-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[MATCH](/sql/t-sql/queries/match-sql-graph?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 和 [PREDICT](/sql/t-sql/queries/predict-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)。 | 是。 不支援 Transact-SQL 查詢子句 [FOR XML](/sql/t-sql/queries/select-for-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[MATCH](/sql/t-sql/queries/match-sql-graph?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[PREDICT](/sql/t-sql/queries/predict-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 和查詢提示。 [OFFSET/FETCH](/sql/t-sql/queries/select-order-by-clause-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#using-offset-and-fetch-to-limit-the-rows-returned) 和 [PIVOT/UNPIVOT](/sql/t-sql/queries/from-using-pivot-and-unpivot?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 只能用來查詢暫存資料表中的資料 (而非外部資料)。 |
| INSERT 陳述式 | 是 | 否 |
| UPDATE 陳述式 | 是 | 否 |
| DELETE 陳述式 | 是 | 否 |
| MERGE 陳述式 | 是 | 否 |
| 資料載入 | 是。 慣用的公用程式是 [COPY](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 陳述式，但是系統同時支援使用 BULK 載入 (BCP) 和 [CETAS](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 來載入資料。 | 否 |
| 資料匯出 | 是。 使用 [CETAS](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)。 | 是。 使用 [CETAS](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)。 |
| 類型 | 是，所有 Transact-SQL 類型，但 [cursor](/sql/t-sql/data-types/cursor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[hierarchyid](/sql/t-sql/data-types/hierarchyid-data-type-method-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[ntext、text 和 image](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[rowversion](/sql/t-sql/data-types/rowversion-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[空間類型](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[sql\_variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 和 [xml](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 除外 | 是，所有 Transact-SQL 類型，但 [cursor](/sql/t-sql/data-types/cursor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[hierarchyid](/sql/t-sql/data-types/hierarchyid-data-type-method-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[ntext、text 和 image](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[rowversion](/sql/t-sql/data-types/rowversion-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[空間類型](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[sql\_variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[xml](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 和資料表類型除外 |
| 跨資料庫查詢 | 否 | 是，包括 [USE](/sql/t-sql/language-elements/use-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 陳述式。 |
| 內建函式 (分析) | 是，所有 Transact-SQL [分析](/sql/t-sql/functions/analytic-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、轉換、[日期和時間](/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、邏輯、[數學](/sql/t-sql/functions/mathematical-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)函式，但 [CHOOSE](/sql/t-sql/functions/logical-functions-choose-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[IIF](/sql/t-sql/functions/logical-functions-iif-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 和 [PARSE](/sql/t-sql/functions/parse-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 除外 | 是，所有 Transact-SQL [分析](/sql/t-sql/functions/analytic-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、轉換、[日期和時間](/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、邏輯、[數學](/sql/t-sql/functions/mathematical-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)函式。 |
| 內建函式 (文字) | 是。 所有 Transact-SQL [字串](/sql/t-sql/functions/string-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[JSON](/sql/t-sql/functions/json-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 和定序函式，但 [STRING_ESCAPE](/sql/t-sql/functions/string-escape-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 和 [TRANSLATE](/sql/t-sql/functions/translate-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 除外 | 是。 所有 Transact-SQL [字串](/sql/t-sql/functions/string-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[JSON](/sql/t-sql/functions/json-functions-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 和定序函式。 |
| 內建資料表值函式 | 是，[Transact-SQL 資料列集函式](/sql/t-sql/functions/functions?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#rowset-functions)，但 [OPENXML](/sql/t-sql/functions/openxml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[OPENDATASOURCE](/sql/t-sql/functions/opendatasource-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[OPENQUERY](/sql/t-sql/functions/openquery-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 和 [OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 除外 | 是，[Transact-SQL 資料列集函式](/sql/t-sql/functions/functions?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#rowset-functions)，但 [OPENXML](/sql/t-sql/functions/openxml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[OPENDATASOURCE](/sql/t-sql/functions/opendatasource-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 和 [OPENQUERY](/sql/t-sql/functions/openquery-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 除外  |
| 彙總 |  Transact-SQL 內建彙總，但 [CHECKSUM_AGG](/sql/t-sql/functions/checksum-agg-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 和 [GROUPING_ID](/sql/t-sql/functions/grouping-id-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 除外 | Transact-SQL 內建彙總，但 [STRING\_AGG](/sql/t-sql/functions/string-agg-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 除外 |
| 操作員 | 是，所有 [Transact-SQL 運算子](/sql/t-sql/language-elements/operators-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)，但 [!>](/sql/t-sql/language-elements/not-greater-than-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 和 [!<](/sql/t-sql/language-elements/not-less-than-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 除外 | 是，所有 [Transact-SQL 運算子](/sql/t-sql/language-elements/operators-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)  |
| 流程控制 | 是。 所有 [Transact-SQL 流程控制陳述式](/sql/t-sql/language-elements/control-of-flow?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)，但 [CONTINUE](/sql/t-sql/language-elements/continue-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[GOTO](/sql/t-sql/language-elements/goto-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[RETURN](/sql/t-sql/language-elements/return-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)、[USE](/sql/t-sql/language-elements/use-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 和 [WAITFOR](/sql/t-sql/language-elements/waitfor-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 除外 | 是。 所有 [Transact-SQL 流程控制陳述式](/sql/t-sql/language-elements/control-of-flow?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)，但 [RETURN](/sql/t-sql/language-elements/return-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 和 `WHILE (...)` 條件中的 SELECT 查詢除外 |
| DDL 陳述式 (CREATE、ALTER、DROP) | 是。 所有適用於所支援物件類型的 Transact-SQL DDL 陳述式 | 是。 所有適用於所支援物件類型的 Transact-SQL DDL 陳述式 |

## <a name="security"></a>安全性

Synapse SQL 可讓您使用內建的安全性功能來保護您的資料和控制存取。 下表會比較 Synapse SQL 耗用量模型之間的高階差異。

|   | 已佈建 | 無伺服器 |
| --- | --- | --- |
| 登入 | N/A (資料庫只支援所包含的使用者) | 是 |
| 使用者 |  N/A (資料庫只支援所包含的使用者) | 是 |
| [自主使用者](/sql/relational-databases/security/contained-database-users-making-your-database-portable?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | 是。 **注意：** 只有一個 AAD 使用者可成為不受限制的系統管理員 | 是 |
| Azure Active Directory (AAD) 驗證 | 是，AAD 使用者 | 是，AAD 登入和使用者 |
| 儲存體 AAD 傳遞驗證 | 是 | 是 |
| 儲存體 SAS 權杖驗證 | 否 | 是，使用 [EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 中的 [DATABASE SCOPED CREDENTIAL](/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 或執行個體層級的 [CREDENTIAL](/sql/t-sql/statements/create-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)。 |
| 儲存體存取金鑰驗證 | 是，使用 [EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) 中的 [DATABASE SCOPED CREDENTIAL](/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | 否 |
| 儲存體受控識別驗證 | 是，使用[受控服務識別認證](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | 是，使用 `Managed Identity` 認證。 |
| 儲存體應用程式身分識別驗證 | [是](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | 否 |
| 權限 - 物件層級 | 是，包括針對使用者 GRANT、DENY 和 REVOKE 權限的能力 | 是，包括在支援的系統物件上針對使用者/登入 GRANT、DENY 和 REVOKE 權限的能力 |
| 權限 - 結構描述層級 | 是，包括在結構描述上針對使用者/登入 GRANT、DENY 和 REVOKE 權限的能力 | 是，包括在結構描述上針對使用者/登入 GRANT、DENY 和 REVOKE 權限的能力 |
| 權限 - [資料庫層級](/sql/relational-databases/security/authentication-access/database-level-roles?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | 是 | 是 |
| 權限 - [伺服器層級](/sql/relational-databases/security/authentication-access/server-level-roles) | 否 | 是，支援 sysadmin 和其他伺服器角色 |
| 角色/群組 | 是 (資料庫範圍) | 是 (伺服器和資料庫範圍) |
| 安全性和身分識別函式 | 某些 Transact-SQL 安全性函式和運算子：`CURRENT_USER`、`HAS_DBACCESS`、`IS_MEMBER`、`IS_ROLEMEMBER`、`SESSION_USER`、`SUSER_NAME`、`SUSER_SNAME`、`SYSTEM_USER`、`USER`、`USER_NAME`、`EXECUTE AS`、`OPEN/CLOSE MASTER KEY` | 某些 Transact-SQL 安全性函式和運算子：`CURRENT_USER`、`HAS_DBACCESS`、`HAS_PERMS_BY_NAME`、`IS_MEMBER', 'IS_ROLEMEMBER`、`IS_SRVROLEMEMBER`、`SESSION_USER`、`SUSER_NAME`、`SUSER_SNAME`、`SYSTEM_USER`、`USER`、`USER_NAME`、`EXECUTE AS` 和 `REVERT`。 安全性函式無法用來查詢外部資料 (請將結果儲存在可用於查詢的變數中)。  |
| DATABASE SCOPED CREDENTIAL | 是 | 是 |

SQL 集區和 SQL 隨選會使用標準的 Transact-SQL 語言來查詢資料。 如需詳細的差異，請參閱 [Transact-SQL 語言參考](/sql/t-sql/language-reference)。

## <a name="tools"></a>工具

您可以使用各種工具來連線到 Synapse SQL 以查詢資料。

|   | 已佈建 | 無伺服器 |
| --- | --- | --- |
| Synapse Studio | 是，SQL 指令碼 | 是，SQL 指令碼 |
| Power BI | 是 | [是](tutorial-connect-power-bi-desktop.md) |
| Azure Analysis Service | 是 | 是 |
| Azure Data Studio | 是 | 是，1.14 版或更高版本。 支援 SQL 指令碼和 SQL Notebook。 |
| SQL Server Management Studio | 是 | 是，18.4 版或更高版本 |

大部分的應用程式都會使用標準的 Transact-SQL 語言，因此都可以查詢 Synapse SQL 的已佈建和無伺服器耗用量模型。

## <a name="storage-options"></a>儲存體選項

所分析的資料可以儲存在各種儲存體類型上。 下表列出所有可用的儲存體選項：

|   | 已佈建 | 無伺服器 |
| --- | --- | --- |
| 內部儲存體 | 是 | 否 |
| Azure Data Lake v2 | 是 | 是 |
| Azure Blob 儲存體 | 是 | 是 |

## <a name="data-formats"></a>資料格式

所分析的資料可使用各種儲存體格式來儲存。 下表列出可以分析的所有可用資料格式：

|   | 已佈建 | 無伺服器 |
| --- | --- | --- |
| Delimited (分隔檔) | [是](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | [是](query-single-csv-file.md) |
| CSV | 是 (不支援多字元分隔符號) | [是](query-single-csv-file.md) |
| Parquet | [是](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | [是](query-parquet-files.md)，包括具有[巢狀型別](query-parquet-nested-types.md)的檔案 |
| Hive ORC | [是](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | 否 |
| Hive RC | [是](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) | 否 |
| JSON | 是 | [是](query-json-files.md) |
| [Delta-lake](https://delta.io/) | 否 | 否 |
| [CDM](https://docs.microsoft.com/common-data-model/) | 否 | 否 |

## <a name="next-steps"></a>後續步驟
如需關於 SQL 集區和 SQL 隨選最佳做法的其他資訊，請參閱下列文章：

- [SQL 集區的最佳做法](best-practices-sql-pool.md)
- [SQL 隨選的最佳做法](best-practices-sql-on-demand.md)