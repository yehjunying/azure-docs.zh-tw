---
title: 深度學習與機器學習服務
titleSuffix: Azure
description: 瞭解深度學習與機器學習服務和人工智慧的關係。 深度學習適用于詐騙偵測、語音 & 臉部辨識、情感分析和時間序列預測等案例。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: lazzeri
author: FrancescaLazzeri
ms.date: 03/05/2020
ms.openlocfilehash: b024010583ba1c6e0ffdf663f7335011ce212bf1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "81414574"
---
# <a name="deep-learning-vs-machine-learning"></a>深度學習與機器學習服務

這篇文章可協助您比較深度學習與機器學習。 您將瞭解兩個概念的比較，以及它們如何符合更廣泛的人工智慧類別。 本文也會說明如何將深度學習套用到真實世界的案例，例如詐騙偵測、語音和臉部辨識、情感分析和時間序列預測。

## <a name="deep-learning-machine-learning-and-ai"></a>深度學習、機器學習和 AI

![關聯性圖表： AI 與機器學習與深度學習的比較](./media/concept-deep-learning-vs-machine-learning/ai-vs-machine-learning-vs-deep-learning.png)

請考慮下列定義以瞭解深度學習與機器學習與 AI 的比較：

- **深度學習**是以人工類神經網路為基礎的機器學習服務子集。 _學習_程式是_深度_的，因為人工類神經網路的結構是由多個輸入、輸出和隱藏層所組成。 每一層都包含將輸入資料轉換成下一個層級可用於特定預測工作之資訊的單位。 感謝此結構，機器可以透過自己的資料處理學習。

- **機器學習**服務是人工智慧的子集，會使用技術（例如深度學習），讓機器使用經驗來改善工作。 _學習_程式是以下列步驟為基礎：

   1. 將資料送入演算法。 （在此步驟中，您可以藉由執行功能解壓縮來提供其他資訊給模型）。
   1. 使用此資料來定型模型。
   1. 測試和部署模型。
   1. 使用已部署的模型來執行自動化預測工作。 （換句話說，請呼叫，並使用已部署的模型來接收模型所傳回的預測）。

- **人工智慧（AI）** 是一種可讓電腦模擬人類智慧的技術。 其中包括機器學習服務。 
 
請務必瞭解 AI、機器學習服務與深度學習之間的關聯性。 機器學習服務是達成人工智慧的一種方式。 藉由使用機器學習和深度學習技術，您可以建立電腦系統和應用程式，以執行通常與人類智慧相關聯的工作。 這些工作包括影像辨識、語音辨識和語言轉譯。

## <a name="techniques-of-deep-learning-vs-machine-learning"></a>深度學習與機器學習的技術 

既然您已大致瞭解機器學習與深度學習，讓我們來比較這兩個技巧。 在機器學習中，演算法需要告訴您如何藉由取用詳細資訊（例如，藉由執行功能解壓縮）來做出精確的預測。 在深度學習中，演算法可以學習如何透過自己的資料處理進行精確的預測，這是因為人工類神經網路結構。

下表將更詳細地比較這兩種技術：

| |所有機器學習服務 |僅深度學習|
|---|---|---|
|  **資料點數目** | 可以使用少量的資料進行預測。 | 需要使用大量定型資料來進行預測。 |
|  **硬體相依性** | 可以在低端機器上工作。 不需要大量的計算能力。 | 取決於高階機器。 它原本就會執行大量的矩陣乘法運算。 GPU 可以有效率地將這些作業優化。 |
|  **特徵化流程** | 需要由使用者正確地識別及建立功能。 | 從資料學習高階功能，並自行建立新功能。 |
|  **學習方法** | 將學習程式分成較小的步驟。 接著，它會將每個步驟的結果合併成一個輸出。 | 藉由以端對端的方式解決問題，在學習過程中移動。 |
|  **執行時間** | 花一點時間來定型，範圍從數秒到幾個小時。 | 訓練通常需要很長的時間，因為深度學習演算法牽涉到許多層級。 |
|  **輸出** | 輸出通常是數值，例如分數或分類。 | 輸出可以有多種格式，例如文字、分數或音效。 |

## <a name="deep-learning-use-cases"></a>深度學習使用案例

由於人工類神經網路結構，深入學習會擅長在非結構化資料（例如影像、音效、影片和文字）中識別模式。 基於這個理由，深度學習會快速地轉換許多產業，包括醫療保健、能源、財務和運輸。 這些產業現在重新思考傳統的商務流程。 

下列段落會說明一些最常見的深度學習應用程式。

### <a name="named-entity-recognition"></a>命名實體辨識

命名實體辨識是深度學習方法，會將一段文字當做輸入，並將其轉換成預先指定的類別。 這種新資訊可以是郵遞區號、日期、產品識別碼。 然後資訊會儲存在結構化架構中，以建立地址清單，或作為身分識別驗證引擎的基準測試。

### <a name="object-detection"></a>物件偵測

深度學習適用于許多物件偵測使用案例。 物件偵測包含兩個部分：影像分類，然後是影像當地語系化。 影像_分類_會識別影像的物件，例如汽車或人員。 影像_當地語系化_會提供這些物件的特定位置。 

物件偵測已用於遊戲、零售、觀光和自我駕駛汽車等產業。

### <a name="image-caption-generation"></a>影像標題產生

如同影像辨識，在影像字幕中，針對指定的影像，系統必須產生描述影像內容的標題。 當您可以偵測並標記相片中的物件時，下一步就是將這些標籤轉換成描述性句子。 

通常，影像字幕應用程式會使用迴旋類神經網路來識別影像中的物件，然後使用週期性的類神經網路，將標籤轉換成一致的句子。

### <a name="machine-translation"></a>機器翻譯

機器翻譯會從一種語言取得單字或句子，並自動將它們轉譯成另一種語言。 機器翻譯已有一段很長的時間，但深度學習會在兩個特定區域中獲得令人印象深刻的結果：自動翻譯文字（以及語音轉換文字）和自動轉譯影像。

使用適當的資料轉換，類神經網路可以瞭解文字、音訊和視覺信號。 電腦轉譯可用來識別較大音訊檔案中的音效程式碼片段，並以文字形式轉譯語音文字或影像。

### <a name="text-analytics"></a>文字分析

以深度學習方法為基礎的文字分析牽涉到分析大量的文字資料（例如，醫療檔或費用收據）、辨識模式，以及建立組織中的已整理和簡潔資訊。

公司使用深度學習來執行文字分析，以偵測內部交易與政府法規的合規性。 另一個常見的例子是保險詐騙：文字分析通常用來分析大量的檔，以辨識保險索賠遭到詐騙的機會。 

## <a name="artificial-neural-networks"></a>人工類神經網路

人工神經網路是由連接的節點層所組成。 深度學習模型會使用具有大量層級的類神經網路。 

下列各節將探索最熱門的人工類神經網路 typologies。

### <a name="feedforward-neural-network"></a>Feedforward 類神經網路

Feedforward 類神經網路是最基本的人工神經網路類型。 在 feedforward 網路中，資訊只會從輸入層向輸出層移動一個方向。 Feedforward 類神經網路會透過一系列的隱藏層來轉換輸入。 每一層都是由一組神經組成，而且每個層都會完全連接到該圖層中的所有神經。 最後一個完全連接的層（輸出層）代表產生的預測。

### <a name="recurrent-neural-network"></a>週期性類神經網路

週期性的類神經網路是廣為使用的人工類神經網路。 這些網路會儲存層的輸出，並將它送回輸入層，以協助預測圖層的結果。 週期性的類神經網路具有絕佳的學習能力。 它們廣泛用於複雜的工作，例如時間序列預測、學習手寫和辨識語言。

### <a name="convolutional-neural-networks"></a>迴旋類神經網路

迴旋類神經網路是特別有效的人工類神經網路，並提供獨特的架構。 圖層會組織成三個維度：寬度、高度和深度。 一層中的神經不會連接到下一層中的所有神經，而是僅限於圖層神經的一個小型區域。 最後的輸出會縮減為沿著深度維度組織的單一機率分數向量。 

迴旋類神經網路已用於影片辨識、影像辨識和推薦系統等區域。

## <a name="next-steps"></a>後續步驟

下列文章說明如何在[Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/?WT.mc_id=docs-article-lazzeri)中使用深度學習技術：

- [使用 TensorFlow 模型來分類手寫數位](https://docs.microsoft.com/azure/machine-learning/how-to-train-tensorflow?WT.mc_id=docs-article-lazzeri)

- [使用 TensorFlow 估計工具和 Keras 來分類手寫數位](https://docs.microsoft.com/azure/machine-learning/how-to-train-keras?WT.mc_id=docs-article-lazzeri)

- [使用 Pytorch 模型分類影像](https://docs.microsoft.com/azure/machine-learning/how-to-train-pytorch?WT.mc_id=docs-article-lazzeri)

- [使用 Chainer 模型來分類手寫數位](https://docs.microsoft.com/azure/machine-learning/how-to-train-ml-models)

此外，也可以使用[Machine Learning 演算法](algorithm-cheat-sheet.md)功能提要來選擇您模型的演算法。
