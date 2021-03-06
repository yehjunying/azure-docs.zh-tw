---
title: Azure 匯入/匯出服務需求 | Microsoft Docs
description: 了解 Azure 匯入/匯出服務的軟體和硬體需求。
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 08/12/2019
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 58997b20c01f33037a5e5e149caa59e1630373ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255310"
---
# <a name="azure-importexport-system-requirements"></a>Azure 匯入/匯出系統需求

本文說明 Azure 匯入/匯出服務的重要需求。 建議您先仔細檢閱資訊，再使用匯入/匯出服務，然後在作業期間，視需要回顧參考。

## <a name="supported-operating-systems"></a>支援的作業系統

若要使用 WAImportExport 工具來準備硬碟，支援下列**支援 BitLocker 磁碟機加密的 64 位元作業系統**。


|平台 |版本 |
|---------|---------|
|Windows     | Windows 7 Enterprise、Windows 7 Ultimate <br> Windows 8 Pro、Windows 8 Enterprise、Windows 8.1 Pro、Windows 8.1 Enterprise <br> Windows 10        |
|Windows Server     |Windows Server 2008 R2 <br> Windows Server 2012、Windows Server 2012 R2         |

## <a name="other-required-software-for-windows-client"></a>Windows 用戶端的其他必要軟體

|平台 |版本 |
|---------|---------|
|.NET Framework    | 4.5.1       |
| BitLocker        |  _          |


## <a name="supported-storage-accounts"></a>支援的儲存體帳戶

Azure 匯入/匯出服務支援下列儲存體帳戶類型：

- 標準通用 v2 儲存帳戶(建議在大多數情況下使用)
- Blob 儲存體帳戶
- 一般用途 v1 儲存體帳戶 (傳統或 Azure Resource Manager 部署)、

如需有關儲存體帳戶的詳細資訊，請參閱 [Azure 儲存體帳戶概觀](storage-account-overview.md)。

每項工作都只能從僅只一個儲存體帳戶收送資料。 換句話說，單一匯入/匯出作業不能跨越多個儲存體帳戶。 如需建立新儲存體帳戶的詳細資訊，請參閱 [如何建立儲存體帳戶](storage-account-create.md)(英文)。

> [!IMPORTANT]
> Azure 匯入匯出服務不支援已啟用[虛擬網路服務終結點](../../virtual-network/virtual-network-service-endpoints-overview.md) 功能的存儲帳戶。 

## <a name="supported-storage-types"></a>支援的儲存體類型

以下是使用 Azure 匯入/匯出服務時支援的儲存類型清單。


|工作 (Job)  |儲存體服務 |支援  |不支援  |
|---------|---------|---------|---------|
|匯入     |  Azure Blob 儲存體 <br><br> Azure 檔案儲存體       | 支援區塊 Blob 與分頁 Blob <br><br> 支援檔案服務          |
|匯出     |   Azure Blob 儲存體       | 支援區塊 Blob、分頁 Blob 及附加 Blob         | 不支援 Azure 檔案服務


## <a name="supported-hardware"></a>支援的硬體

對於 Azure 匯入/匯出服務，您需要支援的磁碟才能複製資料。

### <a name="supported-disks"></a>支援的磁碟

以下是支援與匯入/匯出服務搭配使用的儲存類型清單。


|磁碟類型  |大小  |支援 |
|---------|---------|---------|
|SSD    |   2.5 吋      |第三屆國家國際田貿會議          |
|HDD     |  2.5 吋<br>3.5 吋       |SATA II、SATA III         |

不支援以下磁碟型態:
- USB。
- 帶內置 USB 適配器的外部硬碟。
- 位於外部硬碟外殼內的磁碟。

單一匯入/匯出作業可以有：
- 最多 10 個 HDD/SSD。
- 混用任何尺寸的 HDD/SSD。

您可以將大量的磁碟機分散到多個作業，而且可建立的作業數目並無限制。 若為匯入工作，則只會處理磁碟機上的第一個資料磁碟區。 此資料磁碟區必須以 NTFS 格式化。

當使用 WAImportExport 工具來準備硬碟和複製資料時，您可以使用外接式 USB 轉接器。 大部分現成可用的 USB 3.0 或更新版配接器應都可使用。


## <a name="next-steps"></a>後續步驟

* [設定 WAImportExport 工具](storage-import-export-tool-how-to.md)
* [使用 AzCopy 指令列實用程式傳輸資料](storage-use-azcopy.md)
* [Azure 匯入匯出 REST API 範例 (英文)](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/)
