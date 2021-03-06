---
title: Azure 服務匯流排訊息的安全性控制
description: 評估 Azure 服務匯流排訊息的安全性控制檢查清單
services: service-bus-messaging
ms.service: service-bus-messaging
author: spelluru
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: spelluru
ms.openlocfilehash: af119ef026b70fcb4a56b4f823d20c0e9eddddc8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "75903255"
---
# <a name="security-controls-for-azure-service-bus-messaging"></a>Azure 服務匯流排訊息的安全性控制

本文記載 Azure 服務匯流排訊息內建的安全性控制項。

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>網路

| 安全性控制 | 是/否 | 注意 | 文件 |
|---|---|--|--|
| 服務端點支援| 是（僅限 Premium 層） | 只有[服務匯流排](service-bus-premium-messaging.md)進階層支援 VNet 服務端點。 |  |
| VNet 插入支援| 否 | |  |
| 網路隔離和防火牆支援| 是（僅限 Premium 層） |  |  |
| 強制通道支援| 否 |  |  |

## <a name="monitoring--logging"></a>監視 & 記錄

| 安全性控制 | 是/否 | 注意| 文件 |
|---|---|--|--|
| Azure 監視支援（Log analytics、App insights 等）| 是 | 透過[Azure 監視器和警示](service-bus-metrics-azure-monitor.md)來支援。 |  |
| 控制和管理平面記錄和審核| 是 | 作業記錄可供使用。  | [服務匯流排診斷記錄](service-bus-diagnostic-logs.md) |
| 資料平面記錄和審核| 否 |  |

## <a name="identity"></a>身分識別

| 安全性控制 | 是/否 | 注意| 文件 |
|---|---|--|--|
| 驗證| 是 | 透過[Azure Active Directory 受控服務識別](service-bus-managed-service-identity.md)進行管理。| [服務匯流排驗證和授權](service-bus-authentication-and-authorization.md)。 |
| 授權| 是 | 支援透過[RBAC](authenticate-application.md)和 SAS 權杖進行授權。 | [服務匯流排驗證和授權](service-bus-authentication-and-authorization.md)。 |

## <a name="data-protection"></a>資料保護

| 安全性控制 | 是/否 | 注意 | 文件 |
|---|---|--|--|
| 待用的伺服器端加密： Microsoft 管理的金鑰 |  [是] 表示預設會使用伺服器端靜態加密。 |  |  |
| 待用的伺服器端加密：客戶管理的金鑰（BYOK） | 是。 | Azure KeyVault 中的客戶管理金鑰可用來加密待用服務匯流排命名空間上的資料。 | [使用 Azure 入口網站，設定客戶管理的金鑰來加密待用 Azure 服務匯流排資料](configure-customer-managed-key.md)  |
| 資料行層級加密（Azure 資料服務）| N/A | |   |
| 傳輸中的加密（例如 ExpressRoute 加密、VNet 加密中和 VNet VNet 加密）| 是 | 支援標準 HTTPS/TLS 機制。 |   |
| API 呼叫加密| 是 | API 呼叫是透過[Azure Resource Manager](../azure-resource-manager/index.yml)和 HTTPS 進行。 |   |

## <a name="configuration-management"></a>設定管理

| 安全性控制 | 是/否 | 注意| 文件 |
|---|---|--|--|
| 設定管理支援（設定的版本設定等）| 是 | 透過[AZURE RESOURCE MANAGER API](/rest/api/resources/)支援資源提供者版本設定。|   |

## <a name="next-steps"></a>後續步驟

- 深入瞭解[跨 Azure 服務的內建安全性控制](../security/fundamentals/security-controls.md)。
