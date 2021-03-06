---
title: 包含檔案
description: 包含檔案
services: virtual-machines
author: tanmaygore
ms.service: virtual-machines
ms.topic: include
ms.date: 02/06/2020
ms.author: tagore
ms.custom: include file
ms.openlocfilehash: 57469bef7014010164234638f3d059ac96b125cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "78383908"
---
## <a name="what-is-the-time-required-for-migration"></a>遷移所需的時間是多少？

遷移的規劃和執行在很大程度上取決於體系結構的複雜性，可能需要幾個月時間。  

## <a name="what-is-the-definition-of-a-new-customer-on-iaas-vms-classic"></a>IaaS VM（經典）上新客戶的定義是什麼？

在 2020 年 2 月（開始棄用前一個月）中沒有 IaaS VM（經典）的客戶被視為新客戶。 

## <a name="what-is-the-definition-of-an-existing-customer-on-iaas-virtual-machines-classic"></a>IaaS 虛擬機器（經典）上現有客戶的定義是什麼？

在 2020 年 2 月的訂閱中啟動或已停止但分配 IaaS VM （經典）的客戶被視為現有客戶。 只有這些客戶才能在 2023 年 3 月 1 日之前將其 VM 從 Azure 服務管理員遷移到 Azure 資源管理器。 

## <a name="why-am-i-getting-an-error-stating-newclassicvmcreationnotallowedforsubscription"></a>為什麼我收到一個錯誤，指出"新經典VM創建不允許訂閱"？

作為停用流程的一部分，IaaS VM（經典版）不再適用于新客戶。 我們確定您為新客戶，因此您的運營未獲授權。 我們強烈建議使用[ARM 使用 Azure 虛擬機器](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell)。 如果無法使用 ARM 使用 Azure VM，請與支援訂閱白名單聯繫。

## <a name="does-this-migration-plan-affect-any-of-my-existing-services-or-applications-that-run-on-azure-virtual-machines"></a>此移轉計劃是否會影響任何在 Azure 虛擬機器上執行的現有服務或應用程式？ 

直到 2023 年 3 月 1 日 IaaS VM（經典）。 IaaS VM（經典）是全面支援的一般服務。 您可以繼續使用這些資源以擴展您在 Microsoft Azure 上的使用量。 2023 年 3 月 1 日，這些 VM 將完全停用，任何活動或分配的 VM 都將停止&交易。 不會影響其他經典資源，如雲服務（經典）、存儲帳戶（經典）等。   

## <a name="what-happens-to-my-vms-if-i-dont-plan-on-migrating-in-the-near-future"></a>如果我最近沒有移轉的打算，我的 VM 會出現什麼狀況？ 

2023 年 3 月 1 日，IaaS VM （Classic） 將完全停用，任何活動或分配的 VM 都將停止&交易。 為了防止業務影響，我們強烈重新評論，立即開始規劃您的遷移，並在 2023 年 3 月 1 日之前完成遷移。 我們不會棄用現有的經典 API、雲服務和資源模型。 我們想要將 Resource Manager 部署模型所提供的進階功能納入考量，讓移轉變簡單。 我們建議您開始計畫將這些資源遷移到 Azure 資源管理器。 

## <a name="what-does-this-migration-plan-mean-for-my-existing-tooling"></a>對於我現有的工具來說，此移轉計劃有何意義？ 

將您的工具更新為 Resource Manager 部署模型，是您在移轉計畫中必須說明的最重要變更之一。

## <a name="how-long-will-the-management-plane-downtime-be"></a>管理平面的停機時間會持續多久？ 

依您所移轉的資源數目而定。 就較小型部署 (幾十個 VM) 而言，整個移轉作業應該不會超過一小時。 如果是大規模部署 (數百個 VM)，則移轉可能需要花費幾個小時。

## <a name="can-i-roll-back-after-my-migrating-resources-are-committed-in-resource-manager"></a>在 Resource Manager 中認可移轉中的資源之後，是否還可以回復？ 

只要資源處於已準備就緒狀態，您就可以中止移轉。 透過認可作業成功移轉資源之後，即不支援復原。

## <a name="can-i-roll-back-my-migration-if-the-commit-operation-fails"></a>如果認可作業失敗，是否可以將移轉復原？ 

如果認可作業失敗，就無法中止移轉。 所有移轉作業 (包括認可作業) 都是等冪的。 因此，建議您稍後再重試作業。 如果仍然面臨錯誤，請創建一個支援票證。

## <a name="do-i-have-to-buy-another-express-route-circuit-if-i-have-to-use-iaas-under-resource-manager"></a>如果我必須使用 Resource Manager 下的 IaaS，是否必須購買另一條 ExpressRoute 線路？ 

否。 我們最近啟用了[將 ExpressRoute 線路從傳統部署模型移至 Resource Manager 部署模型](../articles/expressroute/expressroute-move.md)的功能。 如果您已有 ExpressRoute 線路，就不需要購買新線路。

## <a name="what-if-i-had-configured-role-based-access-control-policies-for-my-classic-iaas-resources"></a>如果我已為傳統 IaaS 資源設定角色型存取控制原則，該怎麼辦？ 

在移轉期間，資源會從傳統轉換至 Resource Manager。 因此，建議您規劃需要在移轉後進行的 RBAC 原則更新。

## <a name="i-backed-up-my-classic-vms-in-a-vault-can-i-migrate-my-vms-from-classic-mode-to-resource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>我已在保存庫中備份我的傳統 VM。 我可以將我的 VM 從傳統模式移轉至 Resource Manager 模式，並在復原服務保存庫中保護它們嗎？

當您將 VM 從傳統改為 Resource Manager 模式時，在移轉之前所建立的備份將不會遷移至剛遷移的 Resource Manager VM。 不過，如果您想要保留傳統 VM 的備份，請在移轉之前執行下列步驟。 

1. 在復原服務保存庫中，移至 [受保護的項目]**** 索引標籤，然後選取 VM。 
2. 按一下 [停止保護]。 讓 [刪除相關聯的備份資料]** 選項保持 [未核取]**** 狀態。

> [!NOTE]
> 在保留資料之前，您需要一直支付備份執行個體費用。 系統會根據保留範圍剪除備份複本。 不過，最後一個備份複本會永遠保留下來，直到您明確刪除備份資料為止。 建議您檢查虛擬機器的保留範圍，並在超過保留範圍後，對保存庫中的受保護項目核取 [刪除備份資料] 觸發程序。 
>
>

若要將虛擬機器遷移至 Resource Manager 模式： 

1. 從 VM 中刪除備份/快照集擴充功能。
2. 將虛擬機器從傳統模式移轉至 Resource Manager 模式。 確定虛擬機器對應的儲存體和網路資訊也會移轉至 Resource Manager 模式。

此外，如果您想要備份已遷移的 VM，請移至虛擬機器的管理刀鋒視窗來[啟用備份](../articles/backup/quick-backup-vm-portal.md#enable-backup-on-a-vm)。

## <a name="can-i-validate-my-subscription-or-resources-to-see-if-theyre-capable-of-migration"></a>我是否可以驗證訂用帳戶或資源，以查看是否能夠移轉它們？ 

是。 在平台支援的移轉選項中，為移轉做準備的第一個步驟就是驗證資源是否能夠進行移轉。 如果驗證作業失敗，您將會收到無法完成移轉的所有原因相關訊息。

## <a name="what-happens-if-i-run-into-a-quota-error-while-preparing-the-iaas-resources-for-migration"></a>如果我在準備要移轉的 IaaS 資源時遇到配額錯誤，會發生什麼事？ 

建議您中止移轉，然後登錄支援要求，以在您要移轉 VM 的區域中增加配額。 在配額要求獲得核准之後，您便可以開始重新執行移轉步驟。

## <a name="how-do-i-report-an-issue"></a>如何回報問題？ 

請在我們的 [VM 論壇](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows)上，使用關鍵字 ClassicIaaSMigration 來張貼有關移轉的問題和疑惑。 建議您將所有問題都張貼在此論壇上。 如果您持有支援合約，也歡迎您登錄支援票證。

## <a name="what-if-i-dont-like-the-names-of-the-resources-that-the-platform-chose-during-migration"></a>如果我不喜歡平台在移轉期間選擇的資源名稱，該怎麼辦？ 

您在傳統部署模型中明確提供名稱的所有資源，在移轉期間都會獲得保留。 在某些情況下，則會建立新的資源。 例如︰會為每個 VM 建立一個網路介面。 我們目前不支援控制這些在移轉期間建立的新資源名稱。 請在 [Azure 意見反應論壇](https://feedback.azure.com)上針對這項功能進行投票。

## <a name="can-i-migrate-expressroute-circuits-used-across-subscriptions-with-authorization-links"></a>可以透過授權連結移轉跨訂用帳戶使用的 ExpressRoute 線路嗎？ 

無法在不停機的情況下，自動移轉使用跨訂用帳戶授權連結的 ExpressRoute 線路。 我們提供使用手動步驟移轉這些項目的指引。 如需相關步驟和詳細資訊，請參閱[將 ExpressRoute 線路和相關聯的虛擬網路從傳統部署模型移轉至 Resource Manager 部署模型](../articles/expressroute/expressroute-migration-classic-resource-manager.md)。

## <a name="i-got-the-message-vm-is-reporting-the-overall-agent-status-as-not-ready-hence-the-vm-cannot-be-migrated-ensure-that-the-vm-agent-is-reporting-overall-agent-status-as-ready-or-vm-contains-extension-whose-status-is-not-being-reported-from-the-vm-hence-this-vm-cannot-be-migrated"></a>我得到消息 *"VM 報告整個代理狀態為"未就緒"。因此，無法遷移 VM。確保 VM 代理報告的總代理狀態為"就緒"* 或 *"VM 包含其狀態未從 VM 報告擴展"。因此，無法遷移此 VM。*

當 VM 無法連出到網際網路時，就會收到此訊息。 VM 代理程式會使用連出連線來連接到 Azure 儲存體帳戶，來每隔 5 分鐘更新一次代理程式狀態。
