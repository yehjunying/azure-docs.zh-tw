---
title: 將資料磁片連結至 Linux VM
description: 使用入口網站將新的或現有的資料磁碟附加至 Linux VM。
author: cynthn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 07/12/2018
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: 746cef8dfe026c731a677cbf77f729d36342f007
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "78969360"
---
# <a name="use-the-portal-to-attach-a-data-disk-to-a-linux-vm"></a>使用入口網站將資料磁碟附加至 Linux VM 
本文示範如何透過 Azure 入口網站將新的及現有的磁碟連結到 Linux 虛擬機器。 您也可以[在 Azure 入口網站中將資料磁碟連結到 Windows VM](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。 

將磁碟附加至 VM 之前，請檢閱下列提示︰

* 虛擬機器的大小會控制您可以連接的資料磁碟數目。 如需詳細資訊，請參閱[虛擬機器的大小](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。
* 附加至虛擬機器的磁碟實際上是 Azure 中儲存的 .vhd 檔案。 如需詳細資料，請參閱我們的[受控磁碟簡介](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。
* 連接磁碟之後，您必須[連線到 Linux VM 才能掛接新的磁碟](#connect-to-the-linux-vm-to-mount-the-new-disk)。


## <a name="find-the-virtual-machine"></a>尋找虛擬機器
1. 移至 [ [Azure 入口網站](https://portal.azure.com/)] 以尋找 VM。 搜尋並選取 [虛擬機器]  。
2. 從清單中選擇 VM。
3. 在 [**虛擬機器**] 頁面邊欄的 [**設定**] 底下，選擇 [**磁片**]。
   
    ![開啟磁碟設定](./media/attach-disk-portal/find-disk-settings.png)


## <a name="attach-a-new-disk"></a>附加新的磁碟

1. 在 [磁碟]**** 窗格上，按一下 [+ 新增資料磁碟]****。
2. 按一下 [名稱]**** 的下拉式選單，然後選取 [建立磁碟]****：

    ![建立 Azure 受控磁碟](./media/attach-disk-portal/create-new-md.png)

3. 輸入受控磁碟的名稱。 檢閱預設設定，視需要進行更新，然後按一下 [建立]****。
   
   ![檢閱磁碟設定](./media/attach-disk-portal/create-new-md-settings.png)

4. 按一下 [儲存]**** 以建立受控磁碟並更新 VM 組態︰

   ![儲存新的 Azure 受控磁碟](./media/attach-disk-portal/confirm-create-new-md.png)

5. 在 Azure 建立磁片並將它連接至虛擬機器之後，新的磁片會列在虛擬機器的磁片設定中的 [**資料磁片**] 底下。 當受控磁碟是最上層資源時，磁碟會出現在資源群組的根目錄︰

   ![資源群組中的 Azure 受控磁碟](./media/attach-disk-portal/view-md-resource-group.png)

## <a name="attach-an-existing-disk"></a>連接現有磁碟
1. 在 [磁碟]**** 窗格上，按一下 [+ 新增資料磁碟]****。
2. 按一下 [名稱]**** 的下拉式選單，以檢視您的 Azure 訂用帳戶可存取的現有受控磁碟清單。 選取要附加的受控磁碟︰

   ![附加現有的 Azure 受控磁碟](./media/attach-disk-portal/select-existing-md.png)

3. 按一下 [儲存]**** 以附加現有的受控磁碟並更新 VM 組態︰
   
   ![儲存 Azure 受控磁碟更新](./media/attach-disk-portal/confirm-attach-existing-md.png)

4. Azure 將磁碟連接至虛擬機器之後，該磁碟會列在虛擬機器磁碟設定中的 [資料磁碟]**** 下面。

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>連接到 Linux VM 以掛接新磁碟
若要分割、格式化和掛接新磁碟以供 Linux VM 使用，請使用 SSH 登入您的 VM。 如需詳細資訊，請參閱[如何在 Azure 上搭配使用 SSH 與 Linux](mac-create-ssh-keys.md)。 下列範例會以 azureuser 這個使用者名稱，利用 mypublicdns.westus.cloudapp.azure.com 的公用 DNS 項目來連線至 VM****： 

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

在連線到 VM 後，您就可以連結磁碟。 請先使用 `dmesg` (您用來探索新磁碟的方法可能有所不同) 尋找該磁碟。 下列範例會使用 dmesg 來篩選*SCSI*磁片：

```bash
dmesg | grep SCSI
```

輸出類似於下列範例：

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

在這裡，sdc** 是我們想要的磁碟。 

### <a name="partition-a-new-disk"></a>分割新的磁碟
如果您使用現有的磁碟，請略過此步驟直接裝載磁碟。 如果您要安裝新磁碟，您需要分割磁碟。

> [!NOTE]
> 建議您使用可供您的散發版本使用的最新 fdisk 或 parted 版本。

使用 `fdisk` 分割磁碟。 如果磁碟大小為 2 Tebi 位元組 (TiB) 或以上，則您必須使用 GPT 磁碟分割，您可以使用 `parted` 來執行 GPT 磁碟分割。 如果磁碟大小低於 2 TiB，您就能使用 MBR 或 GPT 磁碟分割。 將它設為磁碟分割 1 上的主要磁碟，並接受其他預設值。 下列範例會在 /dev/sdc** 上啟動 `fdisk` 程序：

```bash
sudo fdisk /dev/sdc
```

使用 `n` 命令來新增新的磁碟分割。 在此範例中，我們也會針對主要磁碟分割選擇 `p`，並接受其餘的預設值。 輸出將類似下列範例：

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

輸入 `p` 來列印磁碟分割表格，接著使用 `w` 來將此表格寫入磁碟，然後結束。 輸出應看起來應類似下列範例：

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

現在，使用 `mkfs` 命令將檔案系統寫入至磁碟分割。 指定檔案系統類型和裝置名稱。 下列範例會在前述步驟所建立的 /dev/sdc1 磁碟分割上，建立 ext4 檔案系統****：

```bash
sudo mkfs -t ext4 /dev/sdc1
```

輸出類似於下列範例：

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

#### <a name="alternate-method-using-parted"></a>使用 parted 的替代方法
Fdisk 公用程式需要互動式輸入，因此不適合在自動化腳本中使用。 不過， [parted](https://www.gnu.org/software/parted/)公用程式可以編寫腳本，因此在自動化案例中會有更好的表現。 Parted 公用程式可以用來分割和格式化資料磁片。 在下面的逐步解說中，我們使用新的資料磁片/dev/sdc，並使用[XFS](https://xfs.wiki.kernel.org/) filesystem 將它格式化。
```bash
sudo parted /dev/sdc --script mklabel gpt mkpart xfspart xfs 0% 100%
partprobe /dev/sdc1
```
如上所示，我們會使用[partprobe](https://linux.die.net/man/8/partprobe)公用程式，以確保核心能立即察覺新的磁碟分割和檔案系統。 如果無法使用 partprobe，可能會導致 blkid 或 lslbk 命令不會立即傳回新檔案系統的 UUID。

### <a name="mount-the-disk"></a>裝載磁碟
使用 `mkdir` 建立用來掛接檔案系統的目錄。 下列範例會在 /datadrive** 建立目錄：

```bash
sudo mkdir /datadrive
```

然後，使用 `mount` 掛接檔案系統。 下列範例會將 /dev/sdc1** 磁碟分割掛接至 /datadrive** 掛接點：

```bash
sudo mount /dev/sdc1 /datadrive
```

為了確保重新開機之後自動重新掛接磁碟機，必須將磁碟機新增至 /etc/fstab** 檔案。 此外，強烈建議在 /et/fstab** 中使用全域唯一識別碼 (Universally Unique IDentifier, UUID) 來參考磁碟機，而不只是使用裝置名稱 (例如，/dev/sdc1**)。 如果作業系統在開機期間偵測到磁碟錯誤，使用 UUID 可避免將不正確的磁碟掛接到指定的位置。 其餘的資料磁碟則會被指派這些相同的裝置識別碼。 若要尋找新磁碟機的 UUID，請使用 `blkid` 公用程式：

```bash
sudo -i blkid
```

輸出大致如下列範例所示：

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

> [!NOTE]
> 不當編輯 **/etc/fstab**檔案可能會導致系統無法開機。 如果不確定，請參閱散發套件的文件，以取得如何適當編輯此檔案的相關資訊。 在編輯之前，也建議先備份 /etc/fstab 檔案。

接下來，在文字編輯器中開啟 /etc/fstab** 檔案，如下所示：

```bash
sudo vi /etc/fstab
```

在此範例中，使用先前步驟中所建立之 */dev/sdc1* 裝置的 UUID 值以及 */datadrive* 的掛接點。 在 */etc/fstab* 檔案的結尾加入以下程式碼：

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```
完成時，請儲存 */etc/fstab*檔案，然後重新開機系統。
> [!NOTE]
> 稍後移除資料磁碟而不編輯 fstab，可能會造成 VM 無法開機。 大多數的發行版本會提供 nofail** 和 (或) nobootwait** fstab 選項。 即使磁碟在開機時無法掛接，這些選項也能讓系統開機。 請查閱散發套件的文件，以取得這些參數的相關資訊。
> 
> *Nofail* 選項可確保即使檔案系統已損毀或磁碟在開機時並不存在，仍然會啟動 VM。 如果沒有此選項，您可能會遇到因為[FSTAB 錯誤而無法透過 SSH 連線到 LINUX VM](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)中所述的行為

### <a name="trimunmap-support-for-linux-in-azure"></a>Azure 中 Linux 的 TRIM/UNMAP 支援
有些 Linux 核心會支援 TRIM/UNMAP 作業以捨棄磁碟上未使用的區塊。 此功能主要是在標準儲存體中相當實用，可用來通知 Azure 已刪除的頁面已不再有效而可予以捨棄，而且如果您建立大型檔案，然後再將它們刪除，也可以節省成本。

有兩種方式可在 Linux VM 中啟用 TRIM 支援。 像往常一樣，請參閱您的散發套件以了解建議的方法︰

* 在 /etc/fstab** 中使用 `discard` 掛接選項，例如：

    ```bash
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```
* 在某些情況下，`discard` 選項可能會影響效能。 或者，您也可以從命令列手動執行 `fstrim` 命令，或將它新增到 crontab 來定期執行︰
  
    **Ubuntu**
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    **RHEL/CentOS**

    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="next-steps"></a>後續步驟
您也可以使用 Azure CLI [附加資料磁碟](add-disk.md)。
