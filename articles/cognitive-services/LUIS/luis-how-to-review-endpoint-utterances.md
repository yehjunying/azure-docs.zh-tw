---
title: 審查使用者語句-LUIS
titleSuffix: Azure Cognitive Services
description: 審查由主動式學習所捕捉到的語句，以選取意圖並標記實體以供讀取世界語句;接受變更、定型和發佈。
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/07/2020
ms.author: diberry
ms.openlocfilehash: c976d3b74badc4eeb5978af352fe425089f2fbfb
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/19/2020
ms.locfileid: "83584957"
---
# <a name="how-to-improve-the-luis-app-by-reviewing-endpoint-utterances"></a>如何藉由審查端點語句來改善 LUIS 應用程式

針對正確的預測來審查端點語句的程式稱為[主動式學習](luis-concept-review-endpoint-utterances.md)。 主動式學習會捕捉端點查詢，並選取不確定的使用者端點語句。 您可以檢查這些語句來選取意圖，並標記這些讀取世界語句的實體。 在您的範例語句中接受這些變更，然後進行定型和發佈。 然後，LUIS 會更準確地識別語句。

## <a name="enable-active-learning"></a>啟用主動式學習

若要啟用主動式學習，您必須記錄使用者查詢。 這是藉由呼叫具有 querystring 參數和值的[端點查詢](luis-get-started-create-app.md#query-the-v3-api-prediction-endpoint)來完成 `log=true` 。

使用 LUIS 入口網站來建立正確的端點查詢。

1. 登入[LUIS 入口網站](https://www.luis.ai)，並選取您的**訂**用帳戶和**撰寫資源**，以查看指派給該撰寫資源的應用程式。
1. 在**我的應用程式**] 頁面上選取您的應用程式名稱，以開啟它。
1. 移至 [**管理**] 區段，然後選取 [ **Azure 資源**]。
1. 針對指派的預測資源，選取 [**變更查詢參數**]。

    > [!div class="mx-imgBorder"]
    > ![使用 LUIS 入口網站來儲存主動學習所需的記錄。](./media/luis-tutorial-review-endpoint-utterances/azure-portal-change-query-url-settings.png)

1. 藉由選取 [**完成**] 來切換**儲存記錄**檔，然後再加以儲存。

    > [!div class="mx-imgBorder"]
    > ![使用 LUIS 入口網站來儲存主動學習所需的記錄。](./media/luis-tutorial-review-endpoint-utterances/luis-portal-manage-azure-resource-save-logs.png)

     此動作會藉由新增 `log=true` 查詢字串參數來變更範例 URL。 對執行階段端點進行預測查詢時，請複製並使用已變更的範例查詢 URL。

## <a name="correct-intent-predictions-to-align-utterances"></a>更正意圖預測以對齊語句

每個意圖都有建議的意圖顯示在 [一致的意圖]**** 資料行中。

> [!div class="mx-imgBorder"]
> [![審查 LUIS 不確定的端點語句](./media/label-suggested-utterances/review-endpoint-utterances.png)](./media/label-suggested-utterances/review-endpoint-utterances.png#lightbox)

如果您同意該意圖，請選取核取記號。 如果您不同意該建議，請從一致的意圖下拉式清單中選取正確的意圖，然後選取一致的意圖右邊的核取記號。 在您選取核取記號之後，語句會移至意圖，並從 [**審核端點語句**] 清單中移除。

> [!TIP]
> 請務必移至意圖詳細資料頁面，以從**審核端點語句**清單中的所有範例語句來審查和更正實體預測。

## <a name="delete-utterance"></a>刪除語句

您可以從檢閱清單中刪除每個語句。 刪除後，它就不再出現於清單中。 即使使用者從端點輸入相同意圖，情況也是如此。

如果您不確定是否應該刪除語句，請將它移至 [無] 意圖，或建立新的意圖（例如）， `miscellaneous` 並將語句移至該意圖。

## <a name="disable-active-learning"></a>停用主動式學習

若要停用主動式學習，請不要記錄使用者查詢。 這是藉由使用 querystring 參數和值來設定[端點查詢](luis-get-started-create-app.md#query-the-v2-api-prediction-endpoint) `log=false` ，或不使用 querystring 值來完成，因為預設值為 false。

## <a name="next-steps"></a>後續步驟

若要在您標示建議的語句之後，測試效能有何改善，您可以選取頂端面板中的 [測試]**** 來存取測試主控台。 如需有關如何使用測試主控台來測試應用程式的指示，請參閱[訓練和測試您的應用程式](luis-interactive-test.md)。
