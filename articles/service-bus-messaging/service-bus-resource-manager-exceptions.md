---
title: Azure 服務匯流排 Resource Manager 例外狀況 |Microsoft Docs
description: Azure Resource Manager 和建議的動作所呈現的服務匯流排例外狀況清單。
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: darosa
editor: spelluru
ms.assetid: 3d8526fe-6e47-4119-9f3e-c56d916a98f9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2019
ms.author: aschhab
ms.openlocfilehash: 0f328651ac4422226071d2de12e9cbc787ef64be
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "75978262"
---
# <a name="service-bus-resource-manager-exceptions"></a>服務匯流排 Resource Manager 例外狀況

本文列出使用 Azure Resource Manager 的範本或直接呼叫來與 Azure 服務匯流排互動時所產生的例外狀況。

> [!IMPORTANT]
> 這份檔經常更新。 請回來查看是否有更新。

以下是透過 Azure Resource Manager 呈現的各種例外狀況/錯誤。

## <a name="error-bad-request"></a>錯誤：不正確的要求

「不正確的要求」表示 Resource Manager 收到的要求驗證失敗。

| 錯誤碼 | 錯誤子代碼 | 錯誤訊息 | 描述 | 建議 |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| 不正確的要求 | 40000 | 子代碼 = 40000。 建立佇列時，無法設定屬性 *' property name '* ，因為命名空間 *' namespace name '* 使用「基本」層。 只有「標準」或「Premium」層才支援這項作業。 | 在 Azure 服務匯流排基本層上，無法設定或更新下列屬性- <ul> <li> RequiresDuplicateDetection </li> <li> AutoDeleteOnIdle </li> <li>RequiresSession</li> <li>DefaultMessageTimeToLive </li> <li> DuplicateDetectionHistoryTimeWindow </li> <li> EnableExpress </li> <li> ForwardTo </li> <li> 主題 </li> </ul> | 請考慮從基本升級為標準層或進階層，以使用此功能。 |
| 不正確的要求 | 40000 | 子代碼 = 40000。 無法變更現有佇列（或主題）之 ' requiresDuplicateDetection ' 屬性的值。 | 在建立實體時，必須啟用/停用重複偵測。 無法在建立後變更重複偵測設定參數。 | 若要在先前建立的佇列/主題上啟用重複偵測，您可以使用重複的偵測來建立新的佇列/主題，然後從原始佇列轉送到新的佇列/主題。 |
| 不正確的要求 | 40000 | 子代碼 = 40000。 指定的值16384無效。 屬性 ' MaxSizeInMegabytes ' 必須是下列其中一個值： 1024; 2048; 3072; 4096; 5120。 | MaxSizeInMegabytes 值無效。 | 請確定 MaxSizeInMegabytes 是下列其中一項-1024、2048、3072、4096、5120。 |
| 不正確的要求 | 40000 | 子代碼 = 40000。 無法變更佇列/主題的資料分割。 | 無法變更實體的資料分割。 | 建立新的實體（佇列或主題）並啟用分割區。 | 
| 不正確的要求 | 無 | 命名空間 *' namespace name '* 不存在。 | 命名空間不存在於您的 Azure 訂用帳戶中。 | 若要解決此錯誤，請嘗試下列 <ul> <li> 請確定 Azure 訂用帳戶正確。 </li> <li> 確定命名空間存在。 </li> <li> 請確認命名空間名稱是否正確（沒有任何拼寫錯誤或 null 字串）。 </li> </ul> | 
| 不正確的要求 | 40400 | 子代碼 = 40400。 自動轉送目的地實體不存在。 | 自動轉寄目的地實體的目的地不存在。 | 目的地實體（佇列或主題）必須存在，才會建立來源。 建立目的地實體之後重試。 |
| 不正確的要求 | 40000 | 子代碼 = 40000。 提供的鎖定時間超過允許的最大值 ' 5 ' 分鐘。 | 訊息可以鎖定的時間必須介於1分鐘（最小值）和5分鐘（最大值）之間。 | 請確定提供的鎖定時間介於1分鐘到5分鐘之間。 |
| 不正確的要求 | 40000 | 子代碼 = 40000。 DelayedPersistence 和 RequiresDuplicateDetection 屬性都無法同時啟用。 | 啟用重複偵測的實體必須是持續性的，因此無法延遲持續性。 | 深入瞭解[重複偵測](duplicate-detection.md) |
| 不正確的要求 | 40000 | 子代碼 = 40000。 無法變更現有佇列的 RequiresSession 屬性值。 | 在建立實體時，應該啟用會話的支援。 建立之後，您就無法在現有的實體（佇列或訂用帳戶）上啟用/停用會話 | 刪除並重新建立已啟用 "RequiresSession" 屬性的新佇列（或訂用帳戶）。 |
| 不正確的要求 | 40000 | 子代碼 = 40000。 ' URI_PATH ' 包含服務匯流排不允許的字元。 實體區段只能包含字母、數位、句號（.）、連字號（-）和底線（_）。 | 實體區段只能包含字母、數位、句號（.）、連字號（-）和底線（_）。 任何其他字元都會導致要求失敗。 | 請確定 URI 路徑中沒有不正確字元。 |


## <a name="error-code-429"></a>錯誤碼：429

就像 HTTP 中的「錯誤碼429」指出「要求太多」。 這表示特定資源（命名空間）正在進行節流，因為該資源上有太多要求（或因為作業衝突）。

| 錯誤碼 | 錯誤子代碼 | 錯誤訊息 | 描述 | 建議 |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| 429 | 50004 | 子代碼 = 50004。 要求已終止，因為您的*命名*空間正在進行節流。 | 當傳入要求的數目超過資源的限制時，就會叫用此錯誤狀況。 | 請稍候幾秒鐘，然後再試一次。 <br/> <br/> 深入瞭解[配額](service-bus-quotas.md)和[Azure Resource Manager 要求限制](../azure-resource-manager/management/request-limits-and-throttling.md)|
| 429 | 40901 | 子代碼 = 40901。 另一個衝突的操作正在進行中。 | 相同的資源/實體正在進行另一個衝突的操作 | 等候目前進行中的作業完成，然後再試一次。 |
| 429 | 40900 | 子代碼 = 40900。 衝突。 您要求的作業在資源的目前狀態下不允許。 | 當有多個要求同時對相同的實體（佇列、主題、訂用帳戶或規則）執行作業時，可能會遇到此狀況。 | 請稍候幾秒鐘，然後再試一次 |
| 429 | 40901 | 實體「*機構名稱*」上的要求與另一個要求衝突 | 相同的資源/實體正在進行另一個衝突的操作 | 等待前一個作業完成，然後再試一次 |
| 429 | 40901 | 實體「*機構名稱*」有另一個更新要求正在進行中。 | 相同的資源/實體正在進行另一個衝突的操作 | 等待前一個作業完成，然後再試一次 |
| 429 | 無 | 發生資源衝突。 另一個衝突的作業可能正在進行中。 如果這是失敗作業的重試，背景清除仍處於暫止狀態。 請稍後再試一次。 | 當對相同實體有暫止的作業時，可能會遇到此狀況。 | 等待前一個作業完成，然後再試一次。 |


## <a name="error-code-not-found"></a>錯誤碼：找不到

此錯誤類別表示找不到資源。

| 錯誤碼 | 錯誤子代碼 | 錯誤訊息 | 描述 | 建議 |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| 找不到 | 無 | 找不到實體「*機構名稱*」。 | 找不到作業的實體。 | 請檢查實體是否存在，然後再次嘗試操作。 |
| 找不到 | 無 | 找不到。 作業不存在。 | 您嘗試執行的作業不存在。 | 請檢查操作，然後再試一次。 |
| 找不到 | 無 | 連入要求不會被辨識為命名空間原則 put 要求。 | 傳入要求本文是 null，因此無法以 put 要求的形式執行。 | 請檢查要求本文，以確保它不是 null。 | 
| 找不到 | 無 | 找不到訊息實體「*機構名稱*」。 | 找不到您嘗試用來執行操作的實體。 | 請檢查實體是否存在，然後再次嘗試操作。 |

## <a name="error-code-internal-server-error"></a>錯誤碼：內部伺服器錯誤

此錯誤類別表示發生內部伺服器錯誤

| 錯誤碼 | 錯誤子代碼 | 錯誤訊息 | 描述 | 建議 |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| 內部伺服器錯誤 | 50000 | 子代碼 = 50000。 內部伺服器錯誤| 可能會因各種原因而發生。 其中一些徵兆為- <ul> <li> 用戶端要求/主體已損毀，並導致錯誤。 </li> <li> 用戶端要求因服務上的處理問題而超時。 </li> </ul> | 解決此問題 <ul> <li> 請確定要求參數不是 null 或格式不正確。 </li> <li> 重試要求。 </li> </ul> |

## <a name="error-code-unauthorized"></a>錯誤碼：未經授權

這個錯誤類別表示沒有執行命令的授權。

| 錯誤碼 | 錯誤子代碼 | 錯誤訊息 | 描述 | 建議 |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| 未經授權 | 無 | 次要命名空間上的操作無效。 次要命名空間是唯讀的。 | 已針對次要命名空間執行此作業，這會設定為 readonly 命名空間。 | 請針對主要命名空間重試命令。 深入瞭解[次要命名空間](service-bus-geo-dr.md) |
| 未經授權 | 無 | MissingToken：找不到授權標頭。 | 當授權具有 null 或不正確的值時，就會發生此錯誤。 | 請確定授權標頭中提及的 token 值正確，且不是 null。 |