---
title: 確認關鍵片語擷取的容器實例
titleSuffix: Azure Cognitive Services
description: 瞭解如何驗證關鍵片語擷取的容器實例。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 5c177517ec18d7526b1cc09da74e35cb5434766d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "80876394"
---
### <a name="verify-the-key-phrase-extraction-container-instance"></a>確認關鍵片語擷取的容器實例

1. 選取 [**總覽**] 索引標籤，然後複製 [IP 位址]。
1. 開啟新的瀏覽器索引標籤，並輸入 IP 位址。 例如，輸入`http://<IP-address>:5000 (http://55.55.55.55:5000`）。 容器的首頁隨即顯示，讓您知道容器正在執行。

    ![查看容器首頁以確認它正在執行](../media/how-tos/container-instance/swagger-docs-on-container.png)

1. 選取 [**服務 API 描述**] 連結，以移至容器的 Swagger 頁面。

1. 選擇任何**張貼**api，然後選取 [立即**試用**]。會顯示參數，其中包括下列範例輸入：

    ```json
    {
      "documents": [
        {
          "id": "1",
          "text": "Hello world"
        },
        {
          "id": "2",
          "text": "Bonjour tout le monde"
        },
        {
          "id": "3",
          "text": "La carretera estaba atascada. Había mucho tráfico el día de ayer."
        },
        {
          "id": "4",
          "text": ":) :( :D"
        }
      ]
    }
    ```

1. 使用下列 JSON 內容來取代輸入：

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "7",
          "text": "I was fortunate to attend the KubeCon Conference in Barcelona, it is one of the best conferences I have ever attended. Great people, great sessions and I thoroughly enjoyed it!"
        }
      ]
    }
    ```

1. 將**showStats**設定`true`為。

1. 選取 [**執行**] 以決定文字的情感。

    封裝在容器中的模型會產生範圍從0到1的分數，其中0是負數，而1是正數。

    傳回的 JSON 回應包含更新文字輸入的情感：

    ```json
    {
      "documents": [
        {
          "id": "7",
          "keyPhrases": [
            "Great people",
            "great sessions",
            "KubeCon Conference",
            "Barcelona",
            "best conferences"
          ],
          "statistics": {
            "charactersCount": 176,
            "transactionsCount": 1
          }
        }
      ],
      "errors": [],
      "statistics": {
        "documentsCount": 1,
        "validDocumentsCount": 1,
        "erroneousDocumentsCount": 0,
        "transactionsCount": 1
      }
    }
    ```

我們現在可以將回應裝載`id`的 JSON 資料檔案與原始要求裝載檔`id`相互關聯。 產生的檔具有`keyPhrases`陣列，其中包含已從對應的輸入檔解壓縮的主要片語清單。 此外，每個產生的檔都`characterCount`有`transactionCount`各種統計資料（例如和）。