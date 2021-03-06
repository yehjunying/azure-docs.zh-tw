---
title: 包含檔案
description: 包含檔案
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/04/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 4860dcac666f790fed199536338e50a967113c20
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "76749014"
---
本文提供 Azure 虛擬機器（Vm）可用性功能的總覽。

## <a name="high-availability"></a>高可用性

工作負載通常會分散到不同的虛擬機器，以取得高輸送量和效能，並在 VM 因為更新或其他事件而受到影響時建立冗余。 

Azure 提供幾個選項來達到高可用性。 首先，讓我們來討論一下基本的結構。 

### <a name="availability-zones"></a>可用性區域

[可用性區域](../articles/availability-zones/az-overview.md)可擴充您在 vm 上維護應用程式和資料可用性所需的控制層級。 可用性區域是 Azure 區域內的實體獨立區域。 每個 Azure 地區支援三個可用性區域。 

每個可用性區域各有不同的電力來源、網路和冷卻系統。 將您的解決方案架構為使用區域中複寫的 VM，即可保護您的應用程式和資料免於遭受資料中心損失。 如果有個區域遭到入侵，就可以立即在另一個區域中使用複寫的應用程式和資料。 

![可用性區域](./media/virtual-machines-common-regions-and-availability/three-zones-per-region.png)

深入了解如何在可用性區域中部署 [Windows](../articles/virtual-machines/windows/create-powershell-availability-zone.md) 或[Linux](../articles/virtual-machines/linux/create-cli-availability-zone.md) VM。


### <a name="fault-domains"></a>容錯網域

容錯網域是共用通用電源和網路交換器的基礎硬體邏輯群組，類似於內部部署資料中心內的機架。 

### <a name="update-domains"></a>更新網域

更新網域是可以同時進行維護或重新啟動的基礎硬體邏輯群組。 

此方法可確保當 Azure 平台進行定期維護時，您的應用程式至少有一個執行個體一直保持執行中。 重新開機的更新網域順序在維護期間可能不會連續進行，但一次只會重新開機一個更新網域。


## <a name="virtual-machines-scale-sets"></a>虛擬機器擴展集 

Azure 虛擬機器擴展集可讓您建立和管理一組負載平衡的 Vm。 VM 執行個體的數目可以自動增加或減少，以因應需求或已定義的排程。 擴展集可為您的應用程式提供高可用性，並可讓您集中管理、設定及更新許多 Vm。 我們建議您在擴展集內建立兩個或多個 Vm，以提供高可用性應用程式，並符合[99.95% 的 AZURE SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/)。 擴展集本身不會產生任何費用，您只需支付您所建立的每個 VM 實例。 當單一 VM 是使用 [Azure 進階 SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#premium-ssd) 時，非計劃性的維護事件適用 Azure SLA。 擴展集中的虛擬機器可以跨多個更新網域和容錯網域部署，以最大化可用性和因資料中心中斷而中斷的復原，以及已規劃或未計畫的維護事件。 擴展集中的虛擬機器也可以部署到單一可用性區域，或區域內。 可用性區域部署選項可能會根據協調流程模式而有所不同。

### <a name="preview-orchestration-mode-preview"></a>預覽：協調流程模式預覽
虛擬機器擴展集可讓您指定協調流程模式。  透過虛擬機器擴展集協調流程模式（預覽），您現在可以選擇擴展集是否應協調在擴展集設定模型外明確建立的虛擬機器，或是根據設定模型隱含建立的虛擬機器實例。 選擇 [協調流程模式] VM 協調流程模型可讓您在區域或可用性區域中，將明確定義的虛擬機器群組在一起。 在可用性區域中部署的虛擬機器會將區域隔離提供給 Vm，它們會系結到可用性區域界限，而且不會受到該區域中其他可用性區域可能發生的任何失敗所約束。 

|   | "orchestrationMode"： "VM" （VirtualMachine）| "orchestrationMode": "ScaleSetVM" (VirtualMachineScaleSetVM) |
|----|----|----|
| VM 設定模型| 無。 擴展集模型中未定義 VirtualMachineProfile。 | 必要。 擴展集模型中會填入 VirtualMachineProfile。 |
| 正在將新的 VM 新增至擴展集| 建立 VM 時，會將 Vm 明確新增至擴展集。 | Vm 會根據 VM 設定模型、實例計數和自動調整規則，以隱含方式建立並新增至擴展集。 |
| 可用性區域| 支援區域部署或單一可用性區域中的 Vm| 支援區域部署或多個可用性區域;可以定義區域平衡策略 |
| 容錯網域| 可以定義容錯網域計數。 2或3以區域支援為基礎，而5用於可用性區域。 指派的 VM 容錯網域將會隨 VM 生命週期保存，包括解除配置和重新開機。 | 可以針對非區域部署定義1、2或3個容錯網域，並為可用性區域部署定義5個。 指派的 VM 容錯網域不會保存在 VM 生命週期中，虛擬機器會在配置時指派一個容錯網域。 |
| 更新網域| N/A。 更新網域會自動對應至容錯網域| N/A。 更新網域會自動對應至容錯網域 |

**容錯網域和更新網域**

虛擬機器擴展集會藉由調整容錯網域和更新網域，來簡化高可用性的設計。 您只需要定義擴展集的容錯網域計數。 擴展集可用的容錯網域數目可能會因區域而異。 請參閱[管理 Azure 中的虛擬機器可用性](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability)。


## <a name="availability-sets"></a>可用性設定組
可用性設定組是資料中心內 VM 的邏輯群組，可讓 Azure 了解您應用程式的建置方式，以提供備援和可用性。 建議您在可用性設定組內建立兩個或多個 VM，以提供具高可用性的應用程式，以及符合 [99.95% Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/)。 可用性設定組本身不需要任何成本，您只需針對您建立的每個 VM 執行個體支付費用。 當單一 VM 是使用 [Azure 進階 SSD](../articles/virtual-machines/windows/disks-types.md#premium-ssd) 時，非計劃性的維護事件適用 Azure SLA。

在可用性設定組中，Vm 會自動分散到這些容錯網域。 此方法可限制潛在實體硬體錯誤、網路中斷或電源中斷的影響。

若 VM 使用 [Azure 受控磁碟](../articles/virtual-machines/windows/faq-for-disks.md)，VM 會在使用受管理的可用性設定組時配合使用受控磁碟容錯網域。 此一配合行為可確保連接到 VM 的所有受控磁碟都位於相同的受控磁碟容錯網域。 

在受管理的可用性設定組中只能建立使用受控磁碟的 VM。 受控磁碟容錯網域數目會依區域而異，每個區域會有兩個或三個受控磁碟容錯網域。 閱讀更多適用於 [Linux VM](../articles/virtual-machines/linux/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set) 或 [Windows VM](../articles/virtual-machines/windows/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set) 的這些受控磁碟容錯網域相關資訊。

![受控可用性設定組](./media/virtual-machines-common-manage-availability/md-fd-updated.png)


可用性設定組內的 Vm 也會自動分散到更新網域。 

![可用性設定組](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

## <a name="next-steps"></a>後續步驟
您現在可以開始使用這些可用性和備援功能來建置 Azure 環境。 如需最佳作法資訊，請參閱 [Azure 可用性最佳作法](/azure/architecture/checklist/resiliency-per-service)。

