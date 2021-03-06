---
title: Azure Cosmos DB 查詢語言中的 ST_ISVALID
description: 瞭解 Azure Cosmos DB 中的 SQL 系統函數 ST_ISVALID。
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 8fbddbe82ae13585b8259a66dffaeef8024baf5d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "71349373"
---
# <a name="st_isvalid-azure-cosmos-db"></a>ST_ISVALID （Azure Cosmos DB）
 傳回布林值，指出指定的 GeoJSON Point、Polygon 或 LineString 運算式是否有效。  
  
## <a name="syntax"></a>語法
  
```sql
ST_ISVALID(<spatial_expr>)  
```  
  
## <a name="arguments"></a>引數
  
*spatial_expr*  
   這是一個 GeoJSON 點、多邊形或 LineString 運算式。  
  
## <a name="return-types"></a>傳回類型
  
  傳回布林運算式。  
  
## <a name="examples"></a>範例
  
  下列範例說明如何使用 ST_VALID 檢查點是否有效。  
  
  例如，此點的維度值不在數值 [-90, 90] 的有效範圍內，因此查詢會傳回 False。  
  
  對於多邊形，GeoJSON 規格需要此資料；若要建立一個封閉的形狀，最後一個座標組應該與第一個座標組相同。 多邊形內的點必須以逆時針順序指定。 以順時針順序指定多邊形，代表區域內的反轉。  
  
```sql
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] }) AS b 
```  
  
 以下為結果集。  
  
```json
[{ "b": false }]  
```  

## <a name="next-steps"></a>後續步驟

- [空間函數 Azure Cosmos DB](sql-query-spatial-functions.md)
- [系統函數 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 簡介](introduction.md)
