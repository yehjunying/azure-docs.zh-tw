---
title: Windows 虛擬桌面個人桌面指派類型-Azure
description: 如何設定 Windows 虛擬桌面個人桌面主機集區的指派類型。
services: virtual-desktop
author: HeidiLohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 8451dc14a7ed42aa92f9adbd5ad050936949e302
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612413"
---
# <a name="configure-the-personal-desktop-host-pool-assignment-type"></a>設定個人桌面主機集區指派類型

>[!IMPORTANT]
>此內容適用于具有 Azure Resource Manager Windows 虛擬桌面物件的春季2020更新。 如果您使用的是 Windows 虛擬桌面不含 Azure Resource Manager 物件的2019版，請參閱[這篇文章](./virtual-desktop-fall-2019/configure-host-pool-personal-desktop-assignment-type-2019.md)。
>
> Windows 虛擬桌面春季2020更新目前為公開預覽狀態。 此預覽版本是在沒有服務等級協定的情況下提供，不建議針對生產環境工作負載使用。 可能不支援特定功能，或可能已經限制功能。 
> 如需詳細資訊，請參閱 [Microsoft Azure 預覽版增補使用條款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

您可以設定個人桌面主機集區的指派類型，以調整您的 Windows 虛擬桌面環境，使其更符合您的需求。 在本主題中，我們將示範如何為您的使用者設定自動或直接指派。

>[!NOTE]
> 本文中的指示僅適用于個人桌面主機集區，而非集區主機集區，因為集區主機集區中的使用者不會指派給特定的工作階段主機。

## <a name="prerequisites"></a>Prerequisites

本文假設您已下載並安裝 Windows 虛擬桌面 PowerShell 模組。 如果您尚未這麼做，請依照[設定 PowerShell 模組](powershell-module.md)中的指示進行。

## <a name="configure-automatic-assignment"></a>設定自動指派

自動指派是在 Windows 虛擬桌面環境中建立之新個人桌面主機集區的預設指派類型。 自動指派使用者不需要特定的工作階段主機。

若要自動指派使用者，請先將它們指派給個人桌面主機集區，讓他們可以在其摘要中看見桌面。 當指派的使用者在其摘要中啟動桌面時，如果已連線到主機集區（完成指派程式），則會宣告可用的工作階段主機。

若要將主機集區設定為自動將使用者指派給 Vm，請執行下列 PowerShell Cmdlet：

```powershell
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -PersonalDesktopAssignmentType Automatic
```

若要將使用者指派給個人桌面主機集區，請執行下列 PowerShell Cmdlet：

```powershell
New-AzRoleAssignment -SignInName <userupn> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <appgroupname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
```

## <a name="configure-direct-assignment"></a>設定直接指派

與自動指派不同的是，當您使用直接指派時，必須將使用者指派給個人桌面主機集區和特定的工作階段主機，才能連線到其個人桌面。 如果使用者只指派給沒有工作階段主機指派的主機集區，他們將無法存取資源。

若要設定主機集區以要求將使用者直接指派給工作階段主機，請執行下列 PowerShell Cmdlet：

```powershell
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -PersonalDesktopAssignmentType Direct
```

若要將使用者指派給個人桌面主機集區，請執行下列 PowerShell Cmdlet：

```powershell
New-AzRoleAssignment -SignInName <userupn> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <appgroupname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
```

若要將使用者指派給特定的工作階段主機，請執行下列 PowerShell Cmdlet：

```powershell
Update-AzWvdSessionHost -HostPoolName <hostpoolname> -Name <sessionhostname> -ResourceGroupName <resourcegroupname> -AssignedUser <userupn>
```

## <a name="next-steps"></a>後續步驟

現在您已設定個人桌面指派類型，您可以登入 Windows 虛擬桌面用戶端，在使用者會話中進行測試。 接下來的兩個操作說明會告訴您如何使用您選擇的用戶端連接到會話：

- [與 Windows 桌面用戶端連線](connect-windows-7-and-10.md)
- [與 Web 用戶端連線](connect-web.md)
- [與 Android 用戶端連線](connect-android.md)
- [與 iOS 用戶端連線](connect-ios.md)
- [與 macOS 用戶端連線](connect-macos.md)