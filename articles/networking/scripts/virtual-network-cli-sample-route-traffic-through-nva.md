---
title: Azure CLI 腳本範例-透過網路虛擬裝置路由傳送流量
description: Azure CLI 指令碼範例 - 透過防火牆網路虛擬設備來路由傳送流量。
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: 05581114ce54ed8e92c6457c95f73b20304e419e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "80521519"
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>透過網路虛擬設備來路由傳送流量

此指令碼範例會建立一個具有前端和後端子網路的虛擬網路。 它也會建立一個已啟用 IP 轉送功能的 VM，以在兩個子網路之間路由傳送流量。 執行此指令碼之後，您可以將網路軟體 (例如防火牆應用程式) 部署到 VM。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>範例指令碼


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>清除部署 

執行下列命令來移除資源群組、VM 和所有相關資源。

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>指令碼說明

此指令碼會使用下列命令來建立資源群組、虛擬網路及網路安全性群組。 下表中的每個命令都會連結至命令特定的文件。

| Command | 注意 |
|---|---|
| [az group create](/cli/azure/group) | 建立用來存放所有資源的資源群組。 |
| [az network vnet create](/cli/azure/network/vnet) | 建立 Azure 虛擬網路和前端子網路。 |
| [az network subnet create](/cli/azure/network/vnet/subnet) | 建立後端和 DMZ 子網路。 |
| [az network public-ip create](/cli/azure/network/public-ip) | 建立公用 IP 位址以從網際網路存取 VM。 |
| [az network nic create](/cli/azure/network/nic) | 建立虛擬網路介面並為它啟用 IP 轉送功能。 |
| [az network nsg create](/cli/azure/network/nsg) | 建立網路安全性群組 (NSG)。 |
| [az network nsg rule create](/cli/azure/network/nsg/rule) | 建立允許對 VM 進行 HTTP 和 HTTPS 連接埠輸入的 NSG 規則。 |
| [az network vnet subnet update](/cli/azure/network/vnet/subnet)| 將 NSG 和路由表與子網路建立關聯。 |
| [az network route-table create](/cli/azure/network/route-table#az-network-route-table-create)| 為所有路由建立一個路由表。 |
| [az network route-table route create](/cli/azure/network/route-table/route#az-network-route-table-route-create)| 建立路由，以透過 VM 在子網路與網際網路之間路由傳送流量。 |
| [az vm create](/cli/azure/vm) | 建立虛擬機器，並將 NIC 連結到此虛擬機器。 此命令也會指定要使用的虛擬機器映像和系統管理認證。 |
| [az group delete](/cli/azure/group) | 刪除資源群組及其包含的所有資源。 |

## <a name="next-steps"></a>後續步驟

如需 Azure CLI 的詳細資訊，請參閱 [Azure CLI 文件](/cli/azure)。

您可以在[Azure 網路功能總覽檔](../cli-samples.md)中找到其他的網路 CLI 腳本範例
