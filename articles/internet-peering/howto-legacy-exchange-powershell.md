---
title: 使用 PowerShell 將舊版 Exchange 對等互連轉換成 Azure 資源
titleSuffix: Azure
description: 使用 PowerShell 將舊版 Exchange 對等互連轉換成 Azure 資源
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: eedf87548d62e05d4940911ed3dcd821077acb27
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "81686782"
---
# <a name="convert-a-legacy-exchange-peering-to-an-azure-resource-by-using-powershell"></a>使用 PowerShell 將舊版 Exchange 對等互連轉換成 Azure 資源

本文說明如何使用 PowerShell Cmdlet，將現有的舊版 Exchange 對等互連轉換成 Azure 資源。

如果您想要的話，可以使用 Azure[入口網站](howto-legacy-exchange-portal.md)來完成本指南。

## <a name="before-you-begin"></a>開始之前
* 開始設定之前，請先參閱[必要條件](prerequisites.md)和[Exchange 對等的逐步](walkthrough-exchange-all.md)解說。

### <a name="work-with-azure-powershell"></a>使用 Azure PowerShell
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="convert-a-legacy-exchange-peering-to-an-azure-resource"></a>將舊版 Exchange 對等互連轉換成 Azure 資源

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>登入您的 Azure 帳戶並且選取您的訂用帳戶
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="get-legacy-exchange-peering-for-conversion"></a><a name= get></a>取得用於轉換的舊版交換對等互連
這個範例示範如何在西雅圖對等互連位置取得舊版交換對等互連：

```powershell
$legacyPeering = Get-AzLegacyPeering -Kind Exchange -PeeringLocation "Seattle"
$legacyPeering
```

回應大致如下列範例所示：
```powershell
    Kind                     : Exchange
    PeeringLocation          : Seattle
    PeerAsn                  : 65000
    Connection               : ------------------------
    PeerSessionIPv4Address   : 10.21.31.100
    MicrosoftIPv4Address     : 10.21.31.50
    SessionStateV4           : Established
    MaxPrefixesAdvertisedV4  : 20000
    PeerSessionIPv6Address   : fe01::3e:100
    MicrosoftIPv6Address     : fe01::3e:50
    SessionStateV6           : Established
    MaxPrefixesAdvertisedV6  : 2000
    ConnectionState          : Active
```

### <a name="convert-legacy-peering"></a>轉換舊版對等互連
此命令可以用來將舊版 Exchange 對等互連轉換成 Azure 資源：

```powershell
$legacyPeering[0] | New-AzPeering `
    -Name "SeattleExchangePeering" `
    -ResourceGroupName "PeeringResourceGroup"

```

&nbsp;
> [!IMPORTANT] 
> 當您將舊版對等互連轉換成 Azure 資源時，不支援修改。
&nbsp;

當端對端布建成功完成時，會顯示此範例回應：

```powershell
    Name                     : SeattleExchangePeering
    Kind                     : Exchange
    Sku                      : Basic_Exchange_Free
    PeeringLocation          : Seattle
    PeerAsn                  : 65000
    Connection               : ------------------------
    PeerSessionIPv4Address   : 10.21.31.100
    MicrosoftIPv4Address     : 10.21.31.50
    SessionStateV4           : Established
    MaxPrefixesAdvertisedV4  : 20000
    PeerSessionIPv6Address   : fe01::3e:100
    MicrosoftIPv6Address     : fe01::3e:50
    SessionStateV6           : Established
    MaxPrefixesAdvertisedV6  : 2000
    ConnectionState          : Active
```
## <a name="additional-resources"></a>其他資源
您可以執行下列命令來取得所有參數的詳細描述：

```powershell
Get-Help Get-AzPeering -detailed
```
如需詳細資訊，請參閱[網際網路對等常見問題](faqs.md)。

## <a name="next-steps"></a>後續步驟

* [使用 PowerShell 建立或修改 Exchange 對等互連](howto-exchange-powershell.md)
