---
title: 多元促進式決策樹：模組參考
titleSuffix: Azure Machine Learning
description: 瞭解如何在 Azure Machine Learning 中使用多元促進式決策樹模組，以使用加上標籤的資料來建立分類器。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/19/2020
ms.openlocfilehash: cfe35f81526a729092edf522f693ccd18494d1ec
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "82137819"
---
# <a name="multiclass-boosted-decision-tree"></a>多元促進式決策樹

本文說明 Azure Machine Learning 設計工具（預覽）中的模組。

使用此模組來建立以促進式決策樹演算法為基礎的機器學習模型。

促進式決策樹是一個集團學習方法，其中第二個樹狀結構會針對第一個樹狀結構的錯誤修正，第三個樹狀目錄會更正第一個和第二個樹系的錯誤，依此類推。 預測是以樹狀結構的集團為基礎。

## <a name="how-to-configure"></a>如何設定 

此模組會建立未定型的分類模型。 因為分類是受監督的學習方法，所以您需要一個加上標籤的*資料集*，其中包含所有資料列的值。

您可以使用[訓練模型](././train-model.md)來定型這種類型的模型。 

1.  將多元促進式**決策樹**模組新增至您的管線。

1.  藉由設定 [**建立定型模式]** 選項，指定您要如何訓練模型。

    + **單一參數**：如果您知道要如何設定模型，可以提供一組特定值做為引數。
    
    + **參數範圍**：如果您不確定最佳參數，而且想要執行參數清理，請選取此選項。 選取要逐一查看的值範圍，[微調模型超參數](tune-model-hyperparameters.md)會逐一查看所提供設定的所有可能組合，以判斷產生最佳結果的超參數。  

1. **每個樹狀結構的葉數上限**限制可以在任何樹狀結構中建立的終端機節點數目上限（葉子）。
    
        By increasing this value, you potentially increase the size of the tree and achieve higher precision, at the risk of overfitting and longer training time.
  
1. [**每個分葉節點的樣本數下限**] 表示在樹狀結構中建立任何終端節點（分葉）所需的案例數目。  

         By increasing this value, you increase the threshold for creating new rules. For example, with the default value of 1, even a single case can cause a new rule to be created. If you increase the value to 5, the training data would have to contain at least five cases that meet the same conditions.

1. 學習**速率**會定義學習時的步驟大小。 請輸入介於0和1之間的數位。

         The learning rate determines how fast or slow the learner converges on an optimal solution. If the step size is too large, you might overshoot the optimal solution. If the step size is too small, training takes longer to converge on the best solution.

1. [**樹狀結構數**] 表示要在集團中建立的決策樹總數。 藉由建立多個決策樹，您或許能夠有較佳的涵蓋範圍，但是定型時間會拉長。

1. **亂數字種子**可選擇性地設定非負整數，做為隨機種子值使用。 指定種子可確保跨回合的重現性具有相同的資料和參數。  

         The random seed is set by default to 42. Successive runs using different random seeds can have different results.

1. 將模型定型：

    + 如果您將 [**建立定型模式**] 設定為 [**單一參數**]，請連接已標記的資料集和 [[訓練模型](train-model.md)] 模組。  
  
    + 如果您將 [**建立定型模式**] 設定為 [**參數範圍**]，請連接已加上標籤的資料集，並使用[微調模型超參數](tune-model-hyperparameters.md)來定型模型。  
  
    > [!NOTE]
    > 
    > 如果您將參數範圍傳遞至[定型模型](train-model.md)，它只會使用單一參數清單中的預設值。  
    > 
    > 如果您將一組參數值傳遞至[微調模型超參數](tune-model-hyperparameters.md)模組，當它預期每個參數的設定範圍時，就會忽略這些值，並使用學習模組的預設值。  
    > 
    > 如果您選取 [**參數範圍**] 選項，並輸入任何參數的單一值，則會在整個清除中使用您指定的單一值，即使其他參數在某個範圍的值之間變更也是如此。

## <a name="next-steps"></a>後續步驟

請參閱可用來 Azure Machine Learning 的[模組集合](module-reference.md)。 
