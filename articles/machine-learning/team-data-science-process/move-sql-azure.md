---
title: 將資料移至 Azure SQL Database - Team Data Science Process
description: 將資料從一般檔案（CSV 或 TSV 格式）或儲存在內部部署 SQL Server 中的資料移至 Azure SQL Database。
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: f9a1424f2afe6c5153e208601b21dff9651880a8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "76722453"
---
# <a name="move-data-to-an-azure-sql-database-for-azure-machine-learning"></a>移動資料至 Azure 機器學習的 Azure SQL Database

本文概述從一般檔案（CSV 或 TSV 格式）或儲存在內部部署 SQL Server 中的資料，將資料移至 Azure SQL Database 的選項。 將資料移到雲端的這些工作是 Team Data Science Process 的一部分。

如需概述用來將資料移至機器學習服務的內部部署 SQL Server 之選項的主題，請參閱[移動資料至 Azure 虛擬機器上的 SQL Server](move-sql-server-virtual-machine.md)。

下表摘要說明移動資料至 Azure SQL Database 的選項。

| <b>來源</b> | <b>目的地：Azure SQL Database</b> |
| --- | --- |
| <b>一般檔案 (CSV 或 TSV 格式)</b> |[大量插入 SQL 查詢](#bulk-insert-sql-query) |
| <b>內部部署 SQL Server</b> |1.[匯出至一般檔案](#export-flat-file)<br> 2. [SQL Database 遷移 Wizard](#insert-tables-bcp)<br> 3.[資料庫備份和還原](#db-migration)<br> 4. [Azure Data Factory](#adf) |

## <a name="prerequisites"></a><a name="prereqs"></a>必要條件
此處概述的程序要求您須擁有：

* **Azure 訂**用帳戶。 如果您沒有訂用帳戶，可以註冊[免費試用](https://azure.microsoft.com/pricing/free-trial/)。
* **Azure 儲存體帳戶**。 在本教學課程中，您會使用 Azure 儲存體帳戶來儲存資料。 如果您沒有 Azure 儲存體帳戶，請參閱 [建立儲存體帳戶](../../storage/common/storage-account-create.md) 一文。 建立儲存體帳戶之後，您必須取得用來存取儲存體的帳戶金鑰。 請參閱[管理儲存體帳戶存取金鑰](../../storage/common/storage-account-keys-manage.md)。
* 存取 **Azure SQL Database**。 如果您必須設定 Azure SQL Database， [開始使用 Microsoft Azure SQL Database](../../sql-database/sql-database-get-started.md) 一文中提供如何佈建 Azure SQL Database 之新執行個體的相關資訊。
* 已在本機上安裝和設定 **Azure PowerShell** 。 如需指示，請參閱 [如何安裝和設定 Azure PowerShell](/powershell/azure/overview)。

**資料**：移轉程序會使用 [NYC 計程車資料集](https://chriswhong.com/open-data/foil_nyc_taxi/)進行示範。 在 Azure Blob 儲存體上，NYC 計程車資料集包含且提供有關車程資料與費用的相關資訊：[NYC 計程車資料](https://www.andresmh.com/nyctaxitrips/)。 這些檔案的範例和說明都會在 [NYC 計程車車程資料集說明](sql-walkthrough.md#dataset)中提供。

您可以將上述程序調整為自己的資料集，或者遵循上述步驟使用 NYC 計程車資料集。 若要將 NYC 計程車資料集上傳至您的內部部署 SQL Server 資料庫，請遵循[大量匯入資料至 SQL Server 資料庫](sql-walkthrough.md#dbload)中概述的程序進行。 這些指示適用於 Azure 虛擬機器上的 SQL Server，但將資料上傳至內部部署 SQL Server 的程序是相同的。

## <a name="moving-data-from-a-flat-file-source-to-an-azure-sql-database"></a><a name="file-to-azure-sql-database"></a>將資料從一般檔案來源移動到 Azure SQL Database
一般檔案中的資料（CSV 或 TSV 格式）可以使用大量插入 SQL 查詢移到 Azure SQL Database。

### <a name="bulk-insert-sql-query"></a><a name="bulk-insert-sql-query"></a>大量插入 SQL 查詢
使用 Bulk Insert SQL 查詢之程式的步驟，類似于將資料從一般檔案來源移至 Azure VM 上 SQL Server 的指示。 如需詳細資料，請參閱 [大量插入 SQL 查詢](move-sql-server-virtual-machine.md#insert-tables-bulkquery)。

## <a name="moving-data-from-on-premises-sql-server-to-an-azure-sql-database"></a><a name="sql-on-prem-to-sazure-sql-database"></a>將資料從內部部署 SQL Server 移至 Azure SQL Database
如果來源資料儲存在內部部署 SQL Server 中，將資料移至 Azure SQL Database 有多種可能的情況：

1. [匯出至一般檔案](#export-flat-file)
2. [SQL Database 遷移 Wizard](#insert-tables-bcp)
3. [資料庫備份和還原](#db-migration)
4. [Azure Data Factory](#adf)

前三個步驟與涵蓋這些相同程式的[Azure 虛擬機器上的將資料移至 SQL Server](move-sql-server-virtual-machine.md)中的章節類似。 下列指示會提供該主題的適當章節連結。

### <a name="export-to-flat-file"></a><a name="export-flat-file"></a>匯出至一般檔案
匯出至一般檔案的步驟與[匯出至](move-sql-server-virtual-machine.md#export-flat-file)一般檔案中所涵蓋的指示類似。

### <a name="sql-database-migration-wizard"></a><a name="insert-tables-bcp"></a>SQL Database 移轉精靈
使用 SQL Database 遷移 Wizard 的步驟，與[SQL Database 遷移 wizard](move-sql-server-virtual-machine.md#sql-migration)中涵蓋的指示類似。

### <a name="database-back-up-and-restore"></a><a name="db-migration"></a>資料庫備份和還原
使用資料庫備份和還原的步驟與[資料庫備份和還原](move-sql-server-virtual-machine.md#sql-backup)中所列的指示類似。

### <a name="azure-data-factory"></a><a name="adf"></a>Azure Data Factory
瞭解如何在本主題中使用 Azure Data Factory （ADF）將資料移至 Azure SQL Database，[使用 Azure Data Factory 將資料從內部部署 sql Server 移至 SQL Azure](move-sql-azure-adf.md)。 本主題說明如何使用 ADF，透過 Azure Blob 儲存體將資料從內部部署 SQL Server 資料庫移至 Azure SQL Database。

當需要使用混合式內部部署和雲端來源來持續遷移資料時，請考慮使用 ADF。  ADF 也有助於資料需要轉換的時間，或在遷移期間需要新的商務邏輯。 ADF 允許使用定期管理資料移動的簡易 JSON 指令碼，來進行排程和監視的工作。 ADF 也有其他功能，例如支援複雜作業。
