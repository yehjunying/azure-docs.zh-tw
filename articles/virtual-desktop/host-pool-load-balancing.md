---
title: Windows 虛擬桌面主機集區負載平衡-Azure
description: Windows 虛擬桌面環境的主機集區負載平衡方法。
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 15d50033a1316601dd8c36bd5748c659f4397d66
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612039"
---
# <a name="host-pool-load-balancing-methods"></a>主機集區負載平衡方法

>[!IMPORTANT]
>此內容適用于具有 Azure Resource Manager Windows 虛擬桌面物件的春季2020更新。 如果您使用的是 Windows 虛擬桌面不含 Azure Resource Manager 物件的2019版，請參閱[這篇文章](./virtual-desktop-fall-2019/host-pool-load-balancing-2019.md)。
>
> Windows 虛擬桌面春季2020更新目前為公開預覽狀態。 此預覽版本是在沒有服務等級協定的情況下提供，不建議針對生產環境工作負載使用。 可能不支援特定功能，或可能已經限制功能。 
> 如需詳細資訊，請參閱 [Microsoft Azure 預覽版增補使用條款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

Windows 虛擬桌面支援兩種負載平衡方法。 每個方法都會決定當工作階段主機連線到主機集區中的資源時，將裝載該使用者的會話。

下列是 Windows 虛擬桌面提供的負載平衡方法：

- 廣度優先的負載平衡可讓您將使用者會話平均分散到主機集區中的工作階段主機上。
- 深度優先的負載平衡可讓您將工作階段主機與主機集區中的使用者會話飽和。 第一個會話達到其會話限制閾值後，負載平衡器會將任何新的使用者連線導向主機集區中的下一個工作階段主機，直到達到其限制為止等等。

每個主機集區只能設定一種特定的負載平衡。 不過，不論是哪一種主機集區，這兩種負載平衡方法都會共用下列行為：

- 如果使用者在主機集區中已經有會話，並重新連線到該會話，則負載平衡器會以現有的會話成功將其重新導向至工作階段主機。 即使該工作階段主機的 AllowNewConnections 屬性設定為 False，也會套用此行為。
- 如果使用者在主機集區中還沒有會話，則負載平衡器不會考慮在負載平衡期間，其 AllowNewConnections 屬性設為 False 的工作階段主機。

## <a name="breadth-first-load-balancing-method"></a>廣度優先的負載平衡方法

廣度優先的負載平衡方法可讓您散發使用者連線，以針對此案例進行優化。 這種方法非常適合想要為連線到其集區虛擬桌面環境的使用者提供最佳體驗的組織。

廣度優先方法會先查詢允許新連接的工作階段主機。 此方法接著會選取會話數目最少的工作階段主機。 如果有系結，方法會選取查詢中的第一個工作階段主機。

## <a name="depth-first-load-balancing-method"></a>深度優先的負載平衡方法

深度優先的負載平衡方法可讓您一次對一個工作階段主機進行飽和，以針對此案例進行優化。 這種方法非常適合符合成本效益的組織，想要更精確地控制其為主機集區所配置的虛擬機器數目。

深度優先方法會先查詢允許新連線且尚未超過其最大會話限制的工作階段主機。 方法接著會選取會話數目最多的工作階段主機。 如果有系結，方法會選取查詢中的第一個工作階段主機。