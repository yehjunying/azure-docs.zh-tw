---
title: 使用 Databricks Jar 轉換資料
description: 了解如何藉由執行 Databricks Jar 來處理或轉換資料。
services: data-factory
documentationcenter: ''
ms.assetid: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: abnarain
author: nabhishek
manager: shwang
ms.date: 03/15/2018
ms.openlocfilehash: 6b010000a674e351051c664dd5eeacd40e802439
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "81414618"
---
# <a name="transform-data-by-running-a-jar-activity-in-azure-databricks"></a>在 Azure Databricks 中執行 Jar 活動來轉換資料
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

[Data Factory 管線](concepts-pipelines-activities.md)中的 Azure Databricks Jar 活動會在 Azure Databricks 叢集中執行 Spark Jar 檔案。 本文是以 [資料轉換活動](transform-data.md) 一文為基礎，其中提供資料轉換和支援的轉換活動的一般總覽。Azure Databricks 是用於執行 Apache Spark 的受控平台。

如需此功能的簡介與示範，請觀看下列 11 分鐘長的影片：

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Execute-Jars-and-Python-scripts-on-Azure-Databricks-using-Data-Factory/player]

## <a name="databricks-jar-activity-definition"></a>Databricks Jar 活動定義

以下是 Databricks Jar 活動的 JSON 定義範例：

```json
{
    "name": "SparkJarActivity",
    "type": "DatabricksSparkJar",
    "linkedServiceName": {
        "referenceName": "AzureDatabricks",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "mainClassName": "org.apache.spark.examples.SparkPi",
        "parameters": [ "10" ],
        "libraries": [
            {
                "jar": "dbfs:/docs/sparkpi.jar"
            }
        ]
    }
}

```

## <a name="databricks-jar-activity-properties"></a>Databricks Jar 活動屬性

下表說明 JSON 定義中使用的 JSON 屬性：

|屬性|描述|必要|
|:--|---|:-:|
|名稱|管線中的活動名稱。|是|
|description|說明活動用途的文字。|否|
|type|若是 Databricks Jar 活動，則活動類型是 DatabricksSparkJar。|是|
|linkedServiceName|Jar 活動執行所在之 Databricks 連結服務的名稱。 若要深入瞭解此連結服務，請參閱 [計算連結服務](compute-linked-services.md) 一文。|是|
|mainClassName|類別的完整名稱，該類別包含要執行的 main 方法。 這個類別必須包含在提供做為程式庫的 JAR 中。|是|
|參數|將傳遞至 main 方法的參數。  這是字串陣列。|否|
|程式庫|要在負責執行工作的叢集上，即將安裝的程式庫清單。 可以是 < 字串, 物件 > 陣列|是 (至少有一個包含 mainClassName 方法)|

> [!NOTE]
> **已知問題**-使用相同的[互動式](compute-linked-services.md#example---using-existing-interactive-cluster-in-databricks)叢集執行並行 Databricks Jar 活動時（不需要重新開機叢集），Databricks 中有一個已知的問題，也就是下列活動將使用第一個活動的參數。 因此，會導致不正確的參數傳遞至後續的作業。 若要減輕此問題，請改用[作業](compute-linked-services.md#example---using-new-job-cluster-in-databricks)叢集。 

## <a name="supported-libraries-for-databricks-activities"></a>Databricks 活動支援的程式庫

在上述的 Databricks 活動定義中，您指定以下的程式庫類型： *jar*、*egg*、 *maven*、 *pypi*、 *cran*。

```json
{
    "libraries": [
        {
            "jar": "dbfs:/mnt/libraries/library.jar"
        },
        {
            "egg": "dbfs:/mnt/libraries/library.egg"
        },
        {
            "maven": {
                "coordinates": "org.jsoup:jsoup:1.7.2",
                "exclusions": [ "slf4j:slf4j" ]
            }
        },
        {
            "pypi": {
                "package": "simplejson",
                "repo": "http://my-pypi-mirror.com"
            }
        },
        {
            "cran": {
                "package": "ada",
                "repo": "https://cran.us.r-project.org"
            }
        }
    ]
}

```

如需詳細資訊，請參閱 [Databricks 文件](https://docs.azuredatabricks.net/api/latest/libraries.html#managedlibrarieslibrary) \(英文\)，了解程式庫類型。

## <a name="how-to-upload-a-library-in-databricks"></a>如何在 Databricks 中上傳程式庫

#### <a name="using-databricks-workspace-ui"></a>[使用 Databricks 工作區 UI](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library) \(英文\)

若要取得利用 UI 新增之程式庫的 dbfs 路徑，您可以使用 [Databricks CLI (安裝)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli) \(英文\)。 

使用 UI 時，Jar 程式庫通常會儲存在 dbfs: FileStore/jar。 您可以透過 CLI 來列出所有 Jar 程式庫：databricks fs ls dbfs:/FileStore/job-jars** 



#### <a name="copy-library-using-databricks-cli"></a>[使用 Databricks CLI 來複製程式庫](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#copy-a-file-to-dbfs) \(英文\)
使用 Databricks CLI [(安裝步驟)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli)。 

範例 - 將 JAR 複製到 dbfs：*dbfs cp SparkPi-assembly-0.1.jar dbfs:/docs/sparkpi.jar*
