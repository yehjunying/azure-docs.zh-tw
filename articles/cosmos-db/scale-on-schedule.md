---
title: 使用 Azure Functions 計時器依排程調整 Azure Cosmos DB
description: 瞭解如何使用 PowerShell 和 Azure Functions，在 Azure Cosmos DB 中調整輸送量的變更。
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: mjbrown
ms.openlocfilehash: 68ba40ea212c061fa5c8bbddc47ea0dfc6d8caa4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "75935164"
---
# <a name="scale-azure-cosmos-db-throughput-by-using-azure-functions-timer-trigger"></a>使用 Azure Functions 計時器觸發程式來調整 Azure Cosmos DB 輸送量

Azure Cosmos 帳戶的效能取決於以每秒要求單位（RU/秒）表示的布建輸送量。 布建為第二個數據細微性，且以每小時最高的 RU 為依據計費。 此佈建容量模型可讓服務提供可預測且一致的輸送量、保證低延遲以及高可用性。 這些功能的大部分生產工作負載。 不過，在開發和測試環境中，Azure Cosmos DB 只會在工作時間內使用，您可以在早上相應增加輸送量，並在上班時間後的晚上相應減少規模。

您可以透過[Azure Resource Manager 範本](resource-manager-samples.md)、 [Azure CLI](cli-samples.md)和[PowerShell](powershell-samples-sql.md)、適用于 Core （SQL） API 帳戶，或使用特定語言的 Azure Cosmos DB sdk 來設定輸送量。 使用 Resource Manager 範本，Azure CLI 或 PowerShell 的優點是它們支援所有 Azure Cosmos DB 模型 Api。

## <a name="throughput-scheduler-sample-project"></a>輸送量排程器範例專案

為了簡化依排程調整 Azure Cosmos DB 的程式，我們建立了稱為「 [Azure Cosmos 輸送量](https://github.com/Azure-Samples/azure-cosmos-throughput-scheduler)排程器」的範例專案。 此專案是具有兩個計時器觸發程式的 Azure Functions 應用程式-"ScaleUpTrigger" 和 "ScaleDownTrigger"。 觸發程式會執行 PowerShell 腳本，以設定每個觸發程式中每個資源`resources.json`上所定義的輸送量。 ScaleUpTrigger 設定為以上午8點執行，而 ScaleDownTrigger 設定為在下午6點執行，而且這些時間可以在檔案中`function.json`針對每個觸發程式輕鬆更新。

您可以在本機複製此專案、加以修改，以指定要相應增加和減少的 Azure Cosmos DB 資源，以及要執行的排程。 稍後您可以將它部署在 Azure 訂用帳戶中，並使用受控服務識別搭配[角色型存取控制](role-based-access-control.md)（RBAC）許可權和「Azure Cosmos DB 操作員」角色來保護它，以在您的 Azure Cosmos 帳戶上設定輸送量。

## <a name="next-steps"></a>後續步驟

- 深入瞭解並從[Azure Cosmos DB 輸送量](https://github.com/Azure-Samples/azure-cosmos-throughput-scheduler)排程器下載範例。
