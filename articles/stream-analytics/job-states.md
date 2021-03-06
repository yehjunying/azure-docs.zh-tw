---
title: Azure 串流分析工作狀態
description: 本文說明串流分析作業的四個不同狀態;執行中、已停止、已降級且失敗。
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: fc46b4f24c3c21cda190f80556373991f6c711c3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "82130933"
---
# <a name="azure-stream-analytics-job-states"></a>Azure 串流分析工作狀態

串流分析作業在任何指定的時間可能處於四個狀態之一： [執行中]、[已停止]、[已降級] 或 [失敗]。 您可以在 Azure 入口網站中串流分析工作的 [概觀] 頁面上找到工作狀態。 

| State | 描述 | 建議動作 |
| --- | --- | --- |
| **正在執行** | 您的工作正在 Azure 上執行，讀取來自定義之輸入來源的事件、處理它們，並將結果寫入至設定的輸出接收器。 | 監視[關鍵計量](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor)是追蹤工作效能的最佳做法。 |
| **已停止** | 您的工作已停止且不會處理事件。 | NA | 
| **降級** | 您的輸入和輸出連線可能會發生間歇性的問題。 這些錯誤稱為暫時性錯誤，可能會讓您的作業進入降級的狀態。 串流分析工作會立即嘗試復原那些錯誤，並返回「正在執行」狀態 (在幾分鐘內)。 這些錯誤可能是因為網路問題、其他 Azure 資源的可用性、還原序列化錯誤等而發生。當作業處於降級狀態時，您的作業效能可能會受到影響。| 若要深入了解這些暫時性錯誤的原因，您可以查看[診斷或活動記錄](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs#debugging-using-activity-logs)。 若是在還原序列化錯誤的情況下，建議採取矯正措施來確保事件不是格式錯誤。 如果工作不斷地達到資源使用量限制，請嘗試增加 SU 數量，或[將您的工作平行化](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization)。 在您無法採取任何動作的其他情況下，串流分析將會嘗試復原至 [正在執行]** 狀態。 <br> 您可以使用[浮水印延遲](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor)計量來瞭解這些暫時性錯誤是否會影響您的作業效能。|
| **Failed** | 您的工作發生嚴重錯誤而造成失敗狀態。 無法讀取和處理事件。 造成工作最後處於失敗狀態的常見原因是執行階段錯誤。 | 您可以[設定警示](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#set-up-alerts-in-the-azure-portal)，如此一來當工作進入失敗狀態時，您就會收到通知。 <br> <br>您可以使用[活動和資源記錄](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs#debugging-using-activity-logs)檔進行調試，以找出根本原因並解決問題。|

## <a name="next-steps"></a>後續步驟
* [設定 Azure 串流分析工作的警示](stream-analytics-set-up-alerts.md)
* [串流分析中的可用計量](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-monitoring#metrics-available-for-stream-analytics)
* [使用活動和資源記錄進行疑難排解](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)
