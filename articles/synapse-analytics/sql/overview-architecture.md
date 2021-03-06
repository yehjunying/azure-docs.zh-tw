---
title: Azure Synapse SQL 架構
description: 瞭解 Azure Synapse SQL 如何結合大量平行處理（MPP）與 Azure 儲存體，以達到高效能和擴充性。
services: synapse-analytics
author: mlee3gsd
manager: rothja
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 570e84b3a545736aad6983c7f0d8c0f0296ca589
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "81431809"
---
# <a name="azure-synapse-sql-architecture"></a>Azure Synapse SQL 架構 

本文說明 Synapse SQL 的架構元件。

[!INCLUDE [preview](../includes/note-preview.md)]

## <a name="synapse-sql-architecture-components"></a>Synapse SQL 架構元件

Synapse SQL 會利用相應放大架構，將資料的計算處理散發到多個節點。 計算與儲存體分開，可讓您獨立調整系統中資料的計算。 

針對 SQL 集區，規模單位是計算能力的抽象概念，稱為[資料倉儲單位](resource-consumption-models.md)。 

針對 SQL 隨選安裝，非伺服器，調整會自動完成以配合查詢資源需求。 隨著拓撲隨著時間的改變而增加、移除節點或容錯移轉，它會調整變更，並確保您的查詢具有足夠的資源並成功完成。 例如，下圖顯示使用4個計算節點執行查詢的隨選 SQL。

![Synapse SQL 架構](./media//overview-architecture/sql-architecture.png)

Synapse SQL 使用以節點為基礎的架構。 應用程式會連接併發出 T-sql 命令到控制節點，這是 Synapse SQL 的單一進入點。 

SQL 集區控制節點會利用 MPP 引擎將查詢優化以進行平行處理，然後將作業傳遞到計算節點，以平行方式執行其工作。 

SQL 隨選控制節點會利用分散式查詢處理（DQP）引擎，將使用者查詢的分散式執行，藉由分割成較小的查詢（將在計算節點上執行）來優化和協調。 每個小型查詢稱為「工作」，代表分散式執行單位。 它會從儲存體讀取檔案、從其他工作、群組或從其他工作中取出的資料聯結結果。 

計算節點會在 Azure 儲存體中儲存所有使用者資料，並執行平行查詢。 資料移動服務 (DMS) 是系統層級的內部服務，其會視需要在節點之間移動資料，以平行方式執行查詢並傳回精確的結果。 

在使用分離的儲存體和計算的情況下，使用 Synapse SQL 時，可以受益于獨立大小的計算能力，不論您的儲存體需求如何。 SQL 隨選調整會自動完成，而針對 SQL 集區，您可以：

* 在 SQL 集區（資料倉儲）內增加或縮小計算能力，而不移動資料。
* 暫停計算容量，同時讓資料保持不變，因此您只需支付儲存體的費用。
* 在營運時間期間繼續計算容量。

## <a name="azure-storage"></a>Azure 儲存體

Synapse SQL 會利用 Azure 儲存體來保護使用者資料的安全。 由於您的資料會由 Azure 儲存體儲存和管理，因此您的儲存體耗用量會有個別的費用。 

SQL 隨選可讓您以唯讀方式查詢 data lake 中的檔案，而 SQL 集區則可讓您同時內嵌資料。 將資料內嵌到 SQL 集區時，資料會分區化到散發中 **，以將**系統的效能優化。 當您定義資料表時，可以選擇要用來散發資料的分區化模式。 支援這些分區化模式：

* 雜湊
* 循環配置資源
* 複寫

## <a name="control-node"></a>控制節點

控制節點是架構的大腦。 它是與所有應用程式與連接互動的前端。 

在 SQL 集區中，MPP 引擎會在控制節點上執行，以優化和協調平行查詢。 當您將 T-SQL 查詢提交至 SQL 集區時，控制節點會將它轉換成以平行方式對每個散發執行的查詢。

在 SQL 隨選中，DQP 引擎會在控制節點上執行，藉由將使用者查詢分割成可在計算節點上執行的較小查詢來優化和協調分散式執行。 它也會指派每個節點要處理的檔案集合。

## <a name="compute-nodes"></a>計算節點數量

計算節點提供計算能力。 

在 SQL 集區中，散發會對應到計算節點以進行處理。 當您支付更多計算資源的費用時，集區會將分佈重新對應到可用的計算節點。 計算節點的數目範圍是從1到60，而且是由 SQL 集區的服務層級所決定。 每個計算節點都有會在系統檢視中顯示的節點識別碼。 您可以在名稱開頭為 sys.pdw_nodes 的系統檢視中尋找 node_id 資料行，以查看計算節點識別碼。 如需這些系統檢視的清單，請參閱 [MPP 系統檢視](/sql/relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views?view=azure-sqldw-latest)。

在 SQL 隨選中，會為每個計算節點指派工作和一組要在其上執行工作的檔案。 工作是分散式查詢執行單位，它實際上是提交查詢使用者的一部分。 自動調整會生效，以確保使用了足夠的計算節點來執行使用者查詢。

## <a name="data-movement-service"></a>資料移動服務

資料移動服務（DMS）是 SQL 集區中的資料傳輸技術，可協調計算節點之間的資料移動。 某些查詢需要資料移動，以確保平行查詢會傳回精確的結果。 若資料移動是必要的，DMS 確保正確的資料會到達正確的位置。

> [!VIDEO https://www.youtube.com/embed/PlyQ8yOb8kc]

## <a name="distributions"></a>散發

散發是在 SQL 集區中的分散式資料上執行的平行查詢的基本儲存單位和處理。 當 SQL 集區執行查詢時，會將工作分成60以平行方式執行的較小查詢。 

這 60 個較小查詢中的每一個都會在其中一個資料散發中執行。 每個計算節點都會管理這 60 個散發中的一或多個。 具有最多計算資源的 SQL 集區具有每個計算節點的一個散發。 具有最少計算資源的 SQL 集區會在一個計算節點上具有所有散發。 

## <a name="hash-distributed-tables"></a>雜湊分散式資料表
雜湊分散式資料表可以針對大型資料表上的聯結和彙總提供最高查詢效能。 

為了將資料分區到雜湊分散式資料表，SQL 集區會使用雜湊函式，以決定性的方式將每一列指派給一個散發。 在資料表定義中，其中一個資料行會指定為發佈資料行。 雜湊函式會使用散發資料行中的值，將每個資料列指派給一個散發。

下圖說明完整 (非分散式資料表) 會如何儲存為雜湊分散式資料表。 

![分散式資料表](media//overview-architecture/hash-distributed-table.png "分散式資料表") 

* 每個資料列屬於一種發佈。 
* 具決定性的雜湊演算法會將每個資料列指派給一個發佈。 
* 每個發佈的資料表資料列數目會隨著顯示不同資料表大小而有所不同。

選取散發資料行有效能考量，例如區分、資料扭曲，以及在系統上執行的查詢類型。

## <a name="round-robin-distributed-tables"></a>循環配置資源分散式資料表

循環配置資源資料表是要建立的最簡單資料表，可以在用來作為暫存資料表以供載入使用時提供快速效能。

循環配置資源分散式資料表會在整個資料表中平均散發資料，但不需任何進一步最佳化。 系統會先隨機選擇一個散發，然後將資料列的緩衝區循序指派給散發。 將資料快速載入循環配置資源資料表，但搭配雜湊分散式資料表通常會有較佳的查詢效能。 迴圈配置資源資料表上的聯結需要重新輪換資料，這會花費額外的時間。

## <a name="replicated-tables"></a>複寫資料表
複寫的資料表會為小型資料表提供最快速的查詢效能。

複寫的資料表會在每個計算節點上快取一份完整的資料表複本。 因此，複寫資料表就不需在進行聯結或彙總之前，於計算節點之間傳輸資料。 複寫的資料表最好能與小型資料表搭配使用。 需要額外的儲存體，而且在寫入資料時會產生額外的負擔，這使得大型資料表變得不切實際。 

下圖顯示在每個計算節點上第一次散發時快取的複寫資料表。 

![複寫的資料表](media/overview-architecture/replicated-table.png "複寫的資料表") 

## <a name="next-steps"></a>後續步驟

現在您已瞭解 Synapse SQL 的相關資訊，請瞭解如何快速[建立 sql 集](../quickstart-create-sql-pool.md)區並[載入範例資料](../sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)（./sql-data-warehouse-load-sample-databases.md）。 或者您可以開始[使用 SQL 隨選](../quickstart-sql-on-demand.md)安裝。 如果您不熟悉 Azure，您可能會發現 [Azure 詞彙](../../azure-glossary-cloud-terminology.md) 在您遇到新術語時很有幫助。 
