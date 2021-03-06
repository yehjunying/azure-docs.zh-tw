---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/06/2019
ms.author: rogarana
ms.openlocfilehash: 6cf9be653da2dd587b93724b6e319dc4d20686c2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "81536420"
---
| 資源 | 標準檔案共用 | Premium 檔案共用 |
|----------|---------------|------------------------------------------|
| 檔案共用大小上限 | 沒有最小值;隨用隨付 | 100 GiB;預 |
| 檔案共用的大小上限 | 100 TiB *、5 TiB | 100 TiB |
| 檔案共用中檔案的大小上限 | 1 TiB | 1 TiB |
| 檔案共用中的檔案數目上限 | 沒有限制 | 沒有限制 |
| 每個共用的最大 IOPS | 10000 IOPS *，1000 IOPS | 100,000 IOPS |
| 每個檔案共用的儲存存取原則數目上限 | 5 | 5 |
| 單一檔案共用的目標輸送量 | 最高 300 MiB/秒 *，最高 60 MiB/秒，  | 請參閱 premium 檔案共用輸入和輸出值|
| 單一檔案共用的輸出上限 | 請參閱標準檔案共用目標輸送量 | 最高 6204 MiB/秒 |
| 單一檔案共用的輸入上限 | 請參閱標準檔案共用目標輸送量 | 最高 4136 MiB/秒 |
| 每個檔案的開啟控制代碼數目上限 | 2,000 個開啟控制代碼 | 2,000 個開啟控制代碼 |
| 共用快照集的數目上限 | 200 個共用快照集 | 200 個共用快照集 |
| 物件 (目錄和檔案) 名稱長度上限 | 2048個字元 | 2048個字元 |
| 最大路徑名稱元件 (在路徑 \A\B\C\D 中，每個字母都是元件) | 255 個字元 | 255 個字元 |

\*標準檔案共用上的預設值為 5 TiB，如需如何增加標準檔案共用的詳細資訊，請參閱[啟用和建立大型檔案共用](../articles/storage/files/storage-files-how-to-create-large-file-share.md)，以取得更多 100 TiB。
