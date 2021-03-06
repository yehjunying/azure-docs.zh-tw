---
title: Azure 中 VM 重新啟動或調整大小的問題 | Microsoft 文件
description: 針對在 Azure 中重新啟動或調整現有虛擬機器時的 Resource Manager 部署問題進行疑難排解
services: virtual-machines
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 0756b52d-4f5a-4503-ae45-c00a6a2edcdf
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a6532558107463311c4225b9855bc4cd3f19eed9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "75965616"
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a>針對在 Azure 中重新啟動或調整現有 Windows VM 大小的部署問題進行疑難排解
當您嘗試啟動已停止的 Azure 虛擬機器 (VM)，或調整現有 Azure VM 的大小時，常會遇到的錯誤是配置失敗。 當叢集或區域沒有可用的資源或無法支援所要求的 VM 大小，就會產生此錯誤。

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a>收集活動記錄
若要開始進行排解疑難，請收集活動記錄，以識別與問題相關的錯誤。 下列連結提供此程序的更多詳細資訊：

[檢視部署作業](../../azure-resource-manager/templates/deployment-history.md)

[檢視活動記錄以管理 Azure 資源](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>問題：啟動已停止的 VM 時發生錯誤
您嘗試啟動已停止的 VM，但是發現配置失敗。

### <a name="cause"></a>原因
必須在架設雲端服務的原始叢集上嘗試提出啟動已停止的 VM 要求。 不過，叢集沒有足夠空間可完成要求。

### <a name="resolution"></a>解決方法
* 停止可用性設定組中的所有 VM，然後重新啟動每一部 VM。
  
  1. 按一下 [**資源** > 群組] 資源*群組* > **Resources** >  > **虛擬機器***虛擬*機 > **停止**的*可用性設定* > 組資源。
  2. 所有 VM 都停止後，選取每個已停止的 VM，然後按一下 [開始]。
* 稍後再重試重新啟動要求。

## <a name="issue-error-when-resizing-an-existing-vm"></a>問題：調整現有 VM 的大小時發生錯誤
您嘗試調整現有 VM 的大小，但是發現配置失敗。

### <a name="cause"></a>原因
必須在架設雲端服務的原始叢集上嘗試提出調整 VM 大小的要求。 不過，叢集不支援要求的 VM 大小。

### <a name="resolution"></a>解決方法
* 以較小的 VM 大小重試要求。
* 如果無法變更要求的 VM 的大小︰
  
  1. 停止可用性設定組中的所有 VM。
     
     * 按一下 [**資源** > 群組] 資源*群組* > **Resources** >  > **虛擬機器***虛擬*機 > **停止**的*可用性設定* > 組資源。
  2. 所有 VM 都停止後，將所需 VM 調整為較大的大小。
  3. 選取已調整大小的 VM，按一下 [啟動] ****，然後啟動每個已停止的 VM。

## <a name="next-steps"></a>後續步驟
如果您在 Azure 中建立新的 Windows VM 時遇到問題，請參閱[針對在 Azure 中建立新 Windows 虛擬機器的部署問題進行疑難排解](../windows/troubleshoot-deployment-new-vm.md)。

