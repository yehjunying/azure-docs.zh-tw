---
title: 使用 Azure 虛擬機器擴展集自動升級 OS 映射
description: 瞭解如何在擴展集中的 VM 實例上自動升級 OS 映射
author: avirishuv
ms.author: avverma
ms.topic: conceptual
ms.service: virtual-machine-scale-sets
ms.subservice: management
ms.date: 04/14/2020
ms.reviewer: jushiman
ms.custom: avverma
ms.openlocfilehash: c06ad5ab2688bd62fdf898950a8f64cd655a9fcc
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83124970"
---
# <a name="azure-virtual-machine-scale-set-automatic-os-image-upgrades"></a>Azure 虛擬機器擴展集的 OS 映像自動升級

在擴展集上啟用 OS 映像自動升級，可藉由為擴展集內的所有執行個體安全且自動地升級 OS 磁碟，來協助您簡化更新的管理。

作業系統自動升級具有下列特性︰

- 一旦設定之後，映像發行者所發行的最新作業系統映像便會自動套用至擴展集，完全不需要使用者介入。
- 每次發行者發佈新的映射時，都會以輪流方式升級實例的批次。
- 會與應用程式健康情況探查和[應用程式健康情況擴充功能](virtual-machine-scale-sets-health-extension.md)整合。
- 適用于所有 VM 大小，以及適用于 Windows 和 Linux 映射。
- 您可以隨時選擇不自動升級 (您也可以手動起始作業系統升級)。
- VM 的作業系統磁碟會取代為使用最新映像版本建立的新作業系統磁碟。 系統會執行已設定的延伸模組和自訂資料指令碼，同時保留已保存的資料磁碟。
- 支援[擴充功能排序](virtual-machine-scale-sets-extension-sequencing.md)。
- 可以在任何大小的擴展集上啟用自動 OS 映射升級。

## <a name="how-does-automatic-os-image-upgrade-work"></a>OS 映像自動升級工作進行的方式為何？

升級的方式是使用以最新版映像建立的新磁碟來取代 VM 的 OS 磁碟。 任何已設定的擴充功能和自訂資料指令碼都會在 OS 磁碟上執行，同時保留已保存的資料磁碟。 為了將應用程式停機時間降到最低，升級會分批進行，每次升級不會超過擴展集的 20%。 您也可以整合 Azure Load Balancer 應用程式健康情況探查或[應用程式健康情況擴充功能](virtual-machine-scale-sets-health-extension.md)。 我們的建議是整合應用程式活動訊號，並在升級程序中驗證每個批次的更新是否都成功。

升級程序的運作方式如下︰
1. 在開始進行升級程序之前，協調器會確定整個擴展集內，(因為任何原因而) 狀況不良的執行個體數目未超過 20%。
2. 升級協調器會識別要升級的 VM 實例批次，其中任何一個批次的總實例計數上限為20%，受限於一部虛擬機器的最小批次大小。
3. 所選 VM 實例批次的 OS 磁片會被取代為從最新映射建立的新 OS 磁片。 擴展集模型中的所有指定延伸模組和設定都會套用至升級的實例。
4. 若擴展集已設定應用程式健康情況探查或應用程式健康情況擴充功能，升級作業會先等候 5 分鐘讓執行個體恢復成良好狀態，然後才繼續升級下一批。 如果實例在升級後的5分鐘內未復原其健全狀況，則預設會還原實例的先前 OS 磁片。
5. 升級協調器也會追蹤在升級後變得狀況不良的執行個體百分比。 如果在升級程序進行期間，已升級的執行個體有超過 20% 變成狀況不良，升級作業就會停止。
6. 上述程序會持續進行，直到擴展集中的所有執行個體皆已升級。

擴展集的 OS 升級協調器在升級每個批次之前，都會先檢查整體的擴展集健康情況。 在升級某個批次時，可能會有其他計劃性或非計劃性維護活動也在並行執行，而影響到擴展集執行個體的健康情況。 在此情況下，如果擴展集的執行個體有超過 20% 變得狀況不良，則擴展集升級程序會在當前的批次結束時停止。

## <a name="supported-os-images"></a>支援的作業系統映像
目前僅支援特定的作業系統平台映像。 自訂映射支援是透過[共用映射庫](shared-image-galleries.md)提供自訂映射的[預覽](virtual-machine-scale-sets-automatic-upgrade.md#automatic-os-image-upgrade-for-custom-images-preview)版本。

目前支援下列平臺 Sku （並會定期新增更多）：

| 發行者               | OS 供應項目      |  SKU               |
|-------------------------|---------------|--------------------|
| Canonical               | UbuntuServer  | 16.04-LTS          |
| Canonical               | UbuntuServer  | 18.04-LTS          |
| Rogue Wave (OpenLogic)  | CentOS        | 7.5                |
| CoreOS                  | CoreOS        | 穩定             |
| Microsoft Corporation   | WindowsServer | 2012-R2-Datacenter |
| Microsoft Corporation   | WindowsServer | 2016-Datacenter    |
| Microsoft Corporation   | WindowsServer | 2016-Datacenter-Smalldisk |
| Microsoft Corporation   | WindowsServer | 2016-Datacenter-with-Containers |
| Microsoft Corporation   | WindowsServer | 2019-Datacenter |
| Microsoft Corporation   | WindowsServer | 2019-Datacenter-Smalldisk |
| Microsoft Corporation   | WindowsServer | 2019-Datacenter-with-Containers |
| Microsoft Corporation   | WindowsServer | Datacenter-核心-1903-含-容器-smalldisk |


## <a name="requirements-for-configuring-automatic-os-image-upgrade"></a>設定 OS 映像自動升級的需求

- 映射的*version*屬性必須設定為 [*最新*]。
- 非 Service Fabric 的擴展集必須使用應用程式健康情況探查或[應用程式健康情況擴充功能](virtual-machine-scale-sets-health-extension.md)。
- 使用計算 API 2018-10-01 版或更高版本。
- 請確定擴展集模型中指定的外部資源可供使用且已更新。 範例包括在 VM 擴充功能屬性中用於啟動承載的 SAS URI、儲存體帳戶中的承載，以及模型中祕密的參照等等。
- 針對使用 Windows 虛擬機器的擴展集，從計算 API 版本2019-03-01 開始，擴展集模型定義中的屬性*virtualMachineProfile. osProfile. enableAutomaticUpdates*屬性必須設定為*false* 。 上述屬性可啟用 VM 內升級，其中 "Windows Update" 會套用作業系統修補程式，而不會取代 OS 磁片。 在擴展集上啟用自動 OS 映射升級之後，就不需要透過「Windows Update」進行額外的更新。

### <a name="service-fabric-requirements"></a>Service Fabric 需求

如果您使用 Service Fabric，請確定符合下列條件：
-   Service Fabric[耐久性等級](../service-fabric/service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster)為銀級或金級，而非銅。
-   擴展集模型定義上的 Service Fabric 擴充功能必須具有 TypeHandlerVersion 1.1 或更新版本。
-   持久性層級在 Service Fabric 叢集和擴展集模型定義的 Service Fabric 延伸中應該相同。

請確定 Service Fabric 叢集和 Service Fabric 延伸模組的持久性設定不相符，因為不符合會導致升級錯誤。 您可以根據[此頁面](../service-fabric/service-fabric-cluster-capacity.md#changing-durability-levels)上所述的指導方針來修改持久性層級。


## <a name="automatic-os-image-upgrade-for-custom-images-preview"></a>自訂映射的作業系統映射自動升級（預覽）

> [!IMPORTANT]
> 自訂映射的 OS 映射自動升級目前為公開預覽狀態。 使用以下所述的公開預覽功能時，需要有加入宣告程式。
> 此預覽版本是在沒有服務等級協定的情況下提供，不建議用於生產工作負載。 可能不支援特定功能，或可能已經限制功能。
> 如需詳細資訊，請參閱 [Microsoft Azure 預覽版增補使用條款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

自動 OS 映射升級適用于透過[共用映射資源庫](shared-image-galleries.md)部署的自訂映射。 不支援自動 OS 映射升級的其他自訂映射。

啟用預覽功能需要一次性加入宣告每個訂用帳戶的功能*AutomaticOSUpgradeWithGalleryImage* ，如下所述。

### <a name="rest-api"></a>REST API
下列範例說明如何為您的訂用帳戶啟用預覽：

```
POST on `/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/AutomaticOSUpgradeWithGalleryImage/register?api-version=2015-12-01`
```

功能註冊最多可能需要15分鐘的時間。 若要檢查註冊狀態：

```
GET on `/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/AutomaticOSUpgradeWithGalleryImage?api-version=2015-12-01`
```

為訂用帳戶註冊此功能之後，請將變更傳播到計算資源提供者，以完成加入宣告程式。

```
POST on `/subscriptions/{subscriptionId}/providers/Microsoft.Compute/register?api-version=2019-12-01`
```

### <a name="azure-powershell"></a>Azure PowerShell
使用[AzProviderFeature](/powershell/module/az.resources/register-azproviderfeature) Cmdlet 來啟用訂用帳戶的預覽。

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName AutomaticOSUpgradeWithGalleryImage -ProviderNamespace Microsoft.Compute
```

功能註冊最多可能需要15分鐘的時間。 若要檢查註冊狀態：

```azurepowershell-interactive
Get-AzProviderFeature -FeatureName AutomaticOSUpgradeWithGalleryImage -ProviderNamespace Microsoft.Compute
```

為訂用帳戶註冊此功能之後，請將變更傳播到計算資源提供者，以完成加入宣告程式。

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
使用[az feature register](/cli/azure/feature#az-feature-register)啟用您訂用帳戶的預覽。

```azurecli-interactive
az feature register --namespace Microsoft.Compute --name AutomaticOSUpgradeWithGalleryImage
```

功能註冊最多可能需要15分鐘的時間。 若要檢查註冊狀態：

```azurecli-interactive
az feature show --namespace Microsoft.Compute --name AutomaticOSUpgradeWithGalleryImage
```

為訂用帳戶註冊此功能之後，請將變更傳播到計算資源提供者，以完成加入宣告程式。

```azurecli-interactive
az provider register --namespace Microsoft.Compute
```

### <a name="additional-requirements-for-custom-images"></a>自訂映射的其他需求
- 以上所述的加入宣告程式只需要針對每個訂用帳戶完成一次。 在加入宣告完成後，您可以針對該訂用帳戶中的任何擴展集啟用自動 OS 升級。
- 共用映射資源庫可以位於任何訂用帳戶中，而且不需要另外加入宣告。 只有擴展集訂用帳戶需要功能加入宣告。
- 所有擴展集的自動 OS 映射升級設定程式都相同，如本頁面的設定[一節](virtual-machine-scale-sets-automatic-upgrade.md#configure-automatic-os-image-upgrade)中所述。
- 針對自動 OS 映射升級設定的擴展集實例，會在新版本的映射[發行並複寫](shared-image-galleries.md#replication)至該擴展集的區域時，升級至最新版本的共用映射庫映射。 如果新的映射未複寫至部署規模的區域，擴展集實例將不會升級為最新版本。 區域影像複寫可讓您控制擴展集新映射的推出。
- 不應從該圖庫映射的最新版本中排除新的映射版本。 從資源庫映射的最新版本排除的映射版本，不會透過自動 OS 映射升級向擴展集推出。

> [!NOTE]
>擴展集在設定自動 OS 升級之後，最多需要3小時的時間，才會觸發第一個映射升級首度發行。 這是每個擴展集一次的延遲。 後續的映射推出會在30分鐘內于擴展集上觸發。


## <a name="configure-automatic-os-image-upgrade"></a>設定 OS 映像自動升級
若要設定 OS 映像自動升級，請確定擴展集模型定義中的 *automaticOSUpgradePolicy.enableAutomaticOSUpgrade* 屬性是設為 *true*。

### <a name="rest-api"></a>REST API
下列範例說明如何設定擴展集模型上的 OS 自動升級：

```
PUT or PATCH on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version=2019-12-01`
```

```json
{
  "properties": {
    "upgradePolicy": {
      "automaticOSUpgradePolicy": {
        "enableAutomaticOSUpgrade":  true
      }
    }
  }
}
```

### <a name="azure-powershell"></a>Azure PowerShell
使用[get-azvmss](/powershell/module/az.compute/update-azvmss)指令程式來設定擴展集的自動 OS 映射升級。 下列範例會針對名為*myResourceGroup*的資源群組中名為*myscaleset 擴展集*的擴展集，設定自動升級：

```azurepowershell-interactive
Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -AutomaticOSUpgrade $true
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
使用[az vmss update](/cli/azure/vmss#az-vmss-update)來設定擴展集的自動 OS 映射升級。 使用 Azure CLI 2.0.47 或更新版本。 下列範例會針對名為*myResourceGroup*的資源群組中名為*myscaleset 擴展集*的擴展集，設定自動升級：

```azurecli-interactive
az vmss update --name myScaleSet --resource-group myResourceGroup --set UpgradePolicy.AutomaticOSUpgradePolicy.EnableAutomaticOSUpgrade=true
```

## <a name="using-application-health-probes"></a>使用應用程式健康狀態探查

在作業系統升級期間，擴展集中的 VM 執行個體一次升級一個批次。 只有客戶應用程式在已升級的 VM 執行個體上狀況良好時，升級才應該繼續。 建議應用程式向擴展集的作業系統升級引擎提供健康情況訊號。 根據預設，平台在作業系統升級期間會考慮 VM 電源狀態和延伸模組佈建狀態，以判斷 VM 執行個體在升級之後是否狀況良好。 在 VM 執行個體的作業系統升級期間，VM 執行個體上的作業系統磁碟會根據最新的映像版本，取代為新的磁碟。 作業系統升級完成後，已設定的延伸模組便會在這些 VM 上執行。 只有在執行個體上的所有擴充功能都成功佈建之後，系統才會認為應用程式的狀況良好。

您可以使用應用程式健康情況探查選擇性地設定擴展集，以便為平台提供精確的應用程式持續狀態的相關資訊。 應用程式健康情況探查是當作健康情況訊號使用的自訂負載平衡器探查。 在擴展集 VM 執行個體上執行的應用程式可以回應外部 HTTP 或 TCP 要求，以指出它是否狀況良好。 如需有關自訂負載平衡器探查運作方式的詳細資訊，請參閱[了解負載平衡器探查](../load-balancer/load-balancer-custom-probe-overview.md)。 Service Fabric 擴展集不支援應用程式健康情況探查。 非 Service Fabric 的擴展集需要使用 Load Balancer 應用程式健康情況探查或[應用程式健康情況擴充功能](virtual-machine-scale-sets-health-extension.md)。

如果擴展集設定為使用多個放置群組，則需要用到使用[標準負載平衡器](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview)的探查。

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>將自訂負載平衡器探查設定為擴展集上的應用程式健康情況探查
最佳作法是，針對擴展集健康情況明確地建立負載平衡器探查。 系統可針對現有的 HTTP 探查或 TCP 探查使用相同的端點，但健康情況探查可能會需要不同於傳統負載平衡器探查的行為。 例如，如果執行個體的負載太高，傳統負載平衡器探查可能會傳回狀況不良，但可能不適用於判斷作業系統自動升級期間的執行個體健康情況。 將探查設定為具有不到兩分鐘的高探查率。

您可以在擴展集的 *networkProfile* 中參考負載平衡器探查，而且可以與對內或對外公開的負載平衡器建立關聯，如下所示：

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
}
```

> [!NOTE]
> 搭配 Service Fabric 使用自動 OS 升級時，新的 OS 映像將以更新網域對更新網域的方式推出，以維持在 Service Fabric 中執行之服務的高可用性。 若要在 Service Fabric 中利用自動 OS 升級，您的叢集必須設定為使用銀級耐久性層或更高。 如需 Service Fabric 叢集持久性特性的詳細資訊，請參閱[這份文件](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster)。

### <a name="keep-credentials-up-to-date"></a>讓認證保持在最新狀態
如果您的擴展集使用任何認證來存取外部資源（例如，設定為針對儲存體帳戶使用 SAS 權杖的 VM 擴充功能），請確定認證已更新。 如果有任何認證（包括憑證和權杖）已過期，升級將會失敗，而且第一批 Vm 將會處於失敗狀態。

資源驗證失敗時，復原 VM 並重新啟用自動 OS 升級的建議步驟如下：

* 重新產生傳遞至擴充功能的權杖 (或任何其他認證)。
* 請確認從 VM 內用來向外部實體通訊的任何認證是最新狀態。
* 將擴展集模型中的擴充功能更新為任一新權杖。
* 部署更新的擴展集，這會更新所有 VM 執行個體，包括失敗的 VM 執行個體。

## <a name="using-application-health-extension"></a>使用應用程式健康情況擴充功能
應用程式健康狀態延伸模組會部署在虛擬機器擴展集執行個體內部，並從擴展集執行個體內部報告 VM 健康狀態。 您可以將延伸模組設定為在應用程式端點上探查，並更新該執行個體上的應用程式狀態。 Azure 會檢查此執行個體狀態，以判斷執行個體是否適合進行升級作業。

因為此擴充功能是從 VM 內報告健康情況，所以在無法使用應用程式健康情況探查 (其利用自訂的 Azure Load Balancer [探查](../load-balancer/load-balancer-custom-probe-overview.md)) 等外部探查的情況下，可以使用此擴充功能。

有多種方法可以將應用程式健康情況擴充功能部署至您的擴展集，如[這篇文章](virtual-machine-scale-sets-health-extension.md#deploy-the-application-health-extension)中的範例所詳述。

## <a name="get-the-history-of-automatic-os-image-upgrades"></a>取得 OS 映像自動升級的記錄
您可以使用 Azure PowerShell、Azure CLI 2.0 或 REST API，檢查您擴展集上最近執行之 OS 升級的記錄。 您可以取得過去兩個月內的最近五次 OS 升級嘗試的記錄。

### <a name="rest-api"></a>REST API
下列範例會使用[REST API](/rest/api/compute/virtualmachinescalesets/getosupgradehistory) ，在名為*myResourceGroup*的資源群組中檢查名為*myscaleset 擴展集*的擴展集狀態：

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/osUpgradeHistory?api-version=2019-12-01`
```

GET 呼叫會傳回類似下列範例輸出的內容：

```json
{
    "value": [
        {
            "properties": {
        "runningStatus": {
          "code": "RollingForward",
          "startTime": "2018-07-24T17:46:06.1248429+00:00",
          "completedTime": "2018-04-21T12:29:25.0511245+00:00"
        },
        "progress": {
          "successfulInstanceCount": 16,
          "failedInstanceCount": 0,
          "inProgressInstanceCount": 4,
          "pendingInstanceCount": 0
        },
        "startedBy": "Platform",
        "targetImageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2016-Datacenter",
          "version": "2016.127.20180613"
        },
        "rollbackInfo": {
          "successfullyRolledbackInstanceCount": 0,
          "failedRolledbackInstanceCount": 0
        }
      },
      "type": "Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades",
      "location": "westeurope"
    }
  ]
}
```

### <a name="azure-powershell"></a>Azure PowerShell
使用 [Get-AzVmss](/powershell/module/az.compute/get-azvmss) Cmdlet 來檢查擴展集的 OS 升級歷程記錄。 下列範例會詳細說明如何在名為*myResourceGroup*的資源群組中，檢查名為*myscaleset 擴展集*之擴展集的 OS 升級狀態：

```azurepowershell-interactive
Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -OSUpgradeHistory
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
使用 [az vmss get-os-upgrade-history](/cli/azure/vmss#az-vmss-get-os-upgrade-history) 來檢查擴展集的 OS 升級歷程記錄。 使用 Azure CLI 2.0.47 或更新版本。 下列範例會詳細說明如何在名為*myResourceGroup*的資源群組中，檢查名為*myscaleset 擴展集*之擴展集的 OS 升級狀態：

```azurecli-interactive
az vmss get-os-upgrade-history --resource-group myResourceGroup --name myScaleSet
```

## <a name="how-to-get-the-latest-version-of-a-platform-os-image"></a>如何取得最新版的平台 OS 映像？

您可以使用下列範例，取得自動 OS 升級支援 Sku 的可用映射版本：

### <a name="rest-api"></a>REST API
```
GET on `/subscriptions/subscription_id/providers/Microsoft.Compute/locations/{location}/publishers/{publisherName}/artifacttypes/vmimage/offers/{offer}/skus/{skus}/versions?api-version=2019-12-01`
```

### <a name="azure-powershell"></a>Azure PowerShell
```azurepowershell-interactive
Get-AzVmImage -Location "westus" -PublisherName "Canonical" -Offer "UbuntuServer" -Skus "16.04-LTS"
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
```azurecli-interactive
az vm image list --location "westus" --publisher "Canonical" --offer "UbuntuServer" --sku "16.04-LTS" --all
```

## <a name="manually-trigger-os-image-upgrades"></a>手動觸發 OS 映射升級
在擴展集上啟用自動 OS 映射升級之後，您就不需要在擴展集上手動觸發映射更新。 作業系統升級協調器會自動將最新的可用映射版本套用至您的擴展集實例，而不需要任何手動介入。

針對您不想要等候協調器套用最新映射的特定情況，您可以使用下列範例來手動觸發 OS 映射升級。

> [!NOTE]
> 手動觸發 OS 映射升級並不會提供自動復原功能。 如果實例在升級作業之後無法復原其健全狀況，則無法還原其先前的 OS 磁片。

### <a name="rest-api"></a>REST API
使用[啟動 OS 升級](/rest/api/compute/virtualmachinescalesetrollingupgrades/startosupgrade)API 呼叫來開始輪流升級，以將所有虛擬機器擴展集實例移至最新可用的映射作業系統版本。 已執行最新可用 OS 版本的實例不會受到影響。 下列範例會詳細說明如何在名為*myResourceGroup*的資源群組中，于名為*myscaleset 擴展集*的擴展集上開始滾動 OS 升級：

```
POST on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/osRollingUpgrade?api-version=2019-12-01`
```

### <a name="azure-powershell"></a>Azure PowerShell
使用[AzVmssRollingOSUpgrade](/powershell/module/az.compute/Start-AzVmssRollingOSUpgrade)指令程式來檢查您擴展集的 OS 升級歷程記錄。 下列範例會詳細說明如何在名為*myResourceGroup*的資源群組中，于名為*myscaleset 擴展集*的擴展集上開始滾動 OS 升級：

```azurepowershell-interactive
Start-AzVmssRollingOSUpgrade -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
使用[az vmss 輪流-upgrade start](/cli/azure/vmss/rolling-upgrade#az-vmss-rolling-upgrade-start)來檢查您擴展集的 OS 升級歷程記錄。 使用 Azure CLI 2.0.47 或更新版本。 下列範例會詳細說明如何在名為*myResourceGroup*的資源群組中，于名為*myscaleset 擴展集*的擴展集上開始滾動 OS 升級：

```azurecli-interactive
az vmss rolling-upgrade start --resource-group "myResourceGroup" --name "myScaleSet" --subscription "subscriptionId"
```

## <a name="deploy-with-a-template"></a>透過範本進行部署

您可以使用範本，來為支援的映像 (例如 [Ubuntu 16.04-LTS](https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json)) 部署具有 OS 自動升級功能的擴展集。

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

## <a name="next-steps"></a>後續步驟
如需如何搭配擴展集使用 OS 自動升級的其他範例，請檢閱 [GitHub 存放庫](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade)。
