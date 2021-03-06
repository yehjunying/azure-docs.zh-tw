---
title: Azure 備份中的加密
description: 瞭解 Azure 備份中的加密功能如何協助您保護您的備份資料，並符合您企業的安全性需求。
ms.topic: conceptual
ms.date: 04/30/2020
ms.openlocfilehash: 1f7e0f26df0f8bd70a10b36f658dcfebe897b475
ms.sourcegitcommit: 3beb067d5dc3d8895971b1bc18304e004b8a19b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82765795"
---
# <a name="encryption-in-azure-backup"></a>Azure 備份中的加密

當您使用 Azure 儲存體加密儲存在雲端時，所有備份的資料都會自動加密，以協助您符合安全性和合規性承諾。 此待用資料會使用256位 AES 加密（可用的最強區塊密碼之一）進行加密，且符合 FIPS 140-2 規範。

除了待用加密，傳輸中的所有備份資料都是透過 HTTPS 傳送。 它一律會保留在 Azure 骨幹網路上。

如需詳細資訊，請參閱待用[資料的加密 Azure 儲存體](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)。 請參閱[AZURE 備份常見問題](https://docs.microsoft.com/azure/backup/backup-azure-backup-faq#encryption)，以回答您可能會有關于加密的任何問題。

## <a name="encryption-of-backup-data-using-platform-managed-keys"></a>使用平臺管理的金鑰來加密備份資料

根據預設，您的所有資料都會使用平臺管理的金鑰進行加密。 您不需要從您的端採取任何明確的動作來啟用此加密，並將其套用至您的復原服務保存庫所備份的所有工作負載。

## <a name="encryption-of-backup-data-using-customer-managed-keys"></a>使用客戶管理的金鑰來加密備份資料

備份您的 Azure 虛擬機器時，您現在可以使用由您所擁有和管理的金鑰來加密您的資料。 Azure 備份可讓您使用儲存在 Azure Key Vault 中的 RSA 金鑰來加密您的備份。 用於加密備份的加密金鑰可能與來源所使用的不同。 資料是使用 AES 256 型資料加密金鑰（DEK）來保護，而這也是使用您的金鑰來保護。 這可讓您完全掌控資料和金鑰。 若要允許加密，復原服務保存庫必須被授與 Azure Key Vault 中加密金鑰的存取權。 您可以停用金鑰，或在需要時撤銷存取權。 不過，在您嘗試保護保存庫的任何專案之前，您必須使用金鑰來啟用加密。

>[!NOTE]
>這項功能目前的可用性有限。 如果您想要使用客戶管理的金鑰來加密您的備份資料，請填寫[這份問卷](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0H3_nezt2RNkpBCUTbWEapURE9TTDRIUEUyNFhNT1lZS1BNVDdZVllHWi4u)並寄送電子郵件給我們。 AskAzureBackupTeam@microsoft.com 請注意，使用這項功能的功能可能會受到 Azure 備份服務的核准。

## <a name="backup-of-managed-disk-vms-encrypted-using-customer-managed-keys"></a>使用客戶管理的金鑰來加密的受控磁片 Vm 備份

Azure 備份也可讓您備份使用金鑰進行伺服器端加密的 Azure Vm。 用來加密磁片的金鑰會儲存在 Azure Key Vault 中，並由您管理。 使用客戶管理金鑰的伺服器端加密與 Azure 磁碟加密不同，因為 ADE 會利用 BitLocker （適用于 Windows）和 DM Crypt （適用于 Linux）來執行來賓加密，而 SSE 會加密儲存體服務中的資料，讓您可以針對您的 Vm 使用任何 OS 或映射。 如需詳細資訊，請參閱[使用客戶管理的金鑰加密受控磁片](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption#customer-managed-keys)。

## <a name="backup-of-vms-encrypted-using-ade"></a>使用 ADE 加密的 Vm 備份

使用 Azure 備份，您也可以備份已使用 Azure 磁碟加密加密其 OS 或資料磁片的 Azure 虛擬機器。 ADE 會使用適用于 Windows Vm 的 BitLocker，以及用於 Linux Vm 的 DM Crypt 來執行來賓加密。 如需詳細資訊，請參閱[使用 Azure 備份備份和還原已加密的虛擬機器](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption)。

## <a name="next-steps"></a>後續步驟

- [備份和還原已加密的 Azure VM](backup-azure-vms-encryption.md)
