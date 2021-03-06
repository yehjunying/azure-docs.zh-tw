---
title: 使用 Azure 串流分析的地理柵欄和地理空間匯總
description: 本文說明如何使用 Azure 串流分析進行地理柵欄和地理空間匯總。
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: 5a3aa3786469c3df37b53cb82bdd396871689297
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "75443647"
---
# <a name="geofencing-and-geospatial-aggregation-scenarios-with-azure-stream-analytics"></a>使用 Azure 串流分析的地理柵欄和地理空間匯總案例

使用內建的地理空間函式，您可以使用 Azure 串流分析來建立適用于車隊管理、分享、連線 cars 和資產追蹤等案例的應用程式。

## <a name="geofencing"></a>地理柵欄

Azure 串流分析支援雲端和 IoT Edge 執行時間中的低延遲即時地理柵欄計算。

### <a name="geofencing-scenario"></a>地理柵欄案例

製造公司必須追蹤大樓的資產。 他們配備了具有 GPS 的每個裝置，並想要在裝置離開特定區域時收到通知。

此範例中使用的參考資料，包含每個建築物中所允許之大樓和裝置的地理柵欄資訊。 請記住，參考資料可能是靜態或緩慢變更。 此案例會使用靜態參考資料。 資料的資料流程會持續發出裝置識別碼和其目前的位置。

### <a name="define-geofences-in-reference-data"></a>定義參考資料中的地理柵欄

您可以使用 GeoJSON 物件來定義地理柵欄。 針對相容性版本為1.2 和更高的作業，也可以使用已知的文字（WKT）作為`NVARCHAR(MAX)`來定義地理柵欄。 WKT 是一種開放地理空間協會（OGC）標準，用來以文字格式呈現空間資料。

內建的地理空間函式可以使用定義的地理柵欄，以找出某個元素是否在特定地理柵欄多邊形中。

下表是地理柵欄可儲存在 Azure blob 儲存體或 Azure SQL 資料表中的參考資料的範例。 每個網站都是以地理空間多邊形表示，而每個裝置都與允許的網站識別碼相關聯。

|SiteID|SiteName|地理柵欄|AllowedDeviceID|
|------|--------|--------|---------------|
|1|「Redmond 樓41」|"多邊形（（-122.1337357922017 47.63782998329432、-122.13373042778369 47.637634793257305、-122.13346757130023 47.637642022530954、-122.13348902897235 47.637508280806806、-122.13361777500506 47.637508280806806、-122.13361241058703 47.63732393354484、-122.13265754417773 47.63730947490855、-122.13266290859576 47.637519124743164、-122.13302232460376 47.637515510097955、-122.13301696018573 47.63764925180358、-122.13272728161212 47.63764925180358、-122.13274873928424 47.63784082716388、-122.13373579220172 47.63782998329432））"|位元組|
|2|「Redmond 樓40」|"多邊形（（-122.1336154507967 47.6366745947009、-122.13361008637867 47.636483015064535、-122.13349206918201 47.636479400347675、-122.13349743360004 47.63636372927573、-122.13372810357532 47.63636372927573、-122.13373346799335 47.63617576323771、-122.13263912671528 47.63616491902258、-122.13264985555134 47.63635649982525、-122.13304682248554 47.636367344000604、-122.13305218690357 47.63650831807564、-122.13276250832996 47.636497473929516、-122.13277323716602 47.63668543881025、-122.1336154507967 47.6366745947009））"|為|
|3|「Redmond 樓22」|"多邊形（（-122.13611660248233 47.63758544698554，-122.13635263687564 47.6374083293018，-122.13622389084293 47.63733603619712，-122.13622389084293 47.63717699101473，-122.13581619507266 47.63692757827657，-122.13559625393344 47.637046862778135，-122.13569281345798 47.637144458985965，-122.13570890671207 47.637314348246214，-122.13611660248233 47.63758544698554））"|C|

### <a name="generate-alerts-with-geofence"></a>使用地理柵欄產生警示

裝置可以透過名`DeviceStreamInput`為的串流，每分鐘發出其識別碼和位置。 下表是輸入的資料流程。

|裝置識別碼|GeoPosition|
|--------|-----------|
|為|"POINT （-122.13292341559497 47.636318374032726）"|
|位元組|"POINT （-122.13338475554553 47.63743531308874）"|
|C|"POINT （-122.13354001095752 47.63627622505007）"|

您可以撰寫查詢，將裝置串流與地理柵欄參考資料聯結在一起，並在每次裝置位於允許的建築物外時產生警示。

```SQL
SELECT DeviceStreamInput.DeviceID, SiteReferenceInput.SiteID, SiteReferenceInput.SiteName 
INTO Output
FROM DeviceStreamInput 
JOIN SiteReferenceInput
ON st_within(DeviceStreamInput.GeoPosition, SiteReferenceInput.Geofence) = 0
WHERE DeviceStreamInput.DeviceID = SiteReferenceInput.AllowedDeviceID
```

下圖代表地理柵欄。 您可以看到裝置符合串流資料輸入的位置。

![建立地理柵欄](./media/geospatial-scenarios/building-geofences.png)

裝置 "C" 位於建築物識別碼2內，但不允許根據參考資料。 此裝置應位於建築物識別碼3內部。 執行此作業將會產生此特定違規的警示。

### <a name="site-with-multiple-allowed-devices"></a>具有多個允許裝置的網站

如果網站允許多個裝置，您可以在中`AllowedDeviceID`定義裝置識別碼的陣列，並在`WHERE`子句上使用使用者定義的函式，以確認串流裝置識別碼是否符合該清單中的任何裝置識別碼。 如需詳細資訊，請參閱適用于雲端作業的[JAVASCRIPT udf](stream-analytics-javascript-user-defined-functions.md)教學課程和 edge 作業的[c # udf](stream-analytics-edge-csharp-udf.md)教學課程。

## <a name="geospatial-aggregation"></a>地理空間匯總

Azure 串流分析支援雲端和 IoT Edge 執行時間中的低延遲即時地理空間匯總。

### <a name="geospatial-aggregation-scenario"></a>地理空間匯總案例

Cab 公司想要建立即時應用程式，以引導其 cab 驅動程式尋找目前遇到較高需求的城市區域。

公司會將 city 的邏輯區域儲存為參考資料。 每個區域都是由 Regionid 輸入、RegionName 和地理柵欄所定義。

### <a name="define-the-geofences"></a>定義地理柵欄

下表是地理柵欄可儲存在 Azure blob 儲存體或 Azure SQL 資料表中的參考資料的範例。 每個區域都是以地理空間多邊形表示，用來與來自串流資料的要求相互關聯。

這些多邊形僅供參考，並不代表實際的城市邏輯或實體分離。

|RegionID|RegionName|地理柵欄|
|--------|----------|--------|
|1|辦公|"多邊形（（-74.00279525078275 40.72833625216264，-74.00547745979765 40.721929158663244，-74.00125029839018 40.71893680218994，-73.9957785919998 40.72521409075776，-73.9972377137039 40.72557184584898，-74.00279525078275 40.72833625216264））"|
|2|"Chinatown"|"多邊形（（-73.99712367114876 40.71281582267133，-73.9901070123658 40.71336881907936、-73.99023575839851 40.71452359088633、-73.98976368961189 40.71554823078944、-73.99551434573982 40.717337246783735、-73.99480624255989 40.718491949759304、-73.99652285632942 40.719109951574、-73.99776740131233 40.7168005470334、-73.99903340396736 40.71727219249899、-74.00193018970344 40.71938642421256、-74.00409741458748 40.71688186545551、-74.00051398334358 40.71517415773184、-74.0004281526551 40.714377212470005、-73.99849696216438 40.713450141693166、-73.99748845157478 40.71405192594819、-73.99712367114876 40.71281582267133））"|
|3|Tribeca 有|"多邊形（（-74.01091641815208 40.72583120006787，-74.01338405044578 40.71436586362705，-74.01370591552757 40.713617702123415，-74.00862044723533 40.711308107057235，-74.00194711120628 40.7194238654018，-74.01091641815208 40.72583120006787））"|

### <a name="aggregate-data-over-a-window-of-time"></a>匯總時間範圍內的資料

下表包含 "乘車" 的串流資料。

|UserID|FromLocation|ToLocation|TripRequestedTime|
|------|------------|----------|-----------------|
|為|"POINT （-74.00726861389182 40.71610611981975）"|"POINT （-73.98615095917779 40.703107386025835）"|"2019-03-12T07：00： 00Z"|
|位元組|"POINT （-74.00249841021645 40.723827238895666）"|"POINT （-74.01160699942085 40.71378884930115）"|"2019-03-12T07：01： 00Z"|
|C|"POINT （-73.99680120565864 40.716439898624024）"|"POINT （-73.98289663412544 40.72582343969828）"|"2019-03-12T07：02： 00Z"|
|"D"|"POINT （-74.00741090068288 40.71615626755086）"|"POINT （-73.97999843120539 40.73477895807408）"|"2019-03-12T07：03： 00Z"|

下列查詢會將裝置串流與地理柵欄的參考資料聯結在一起，並計算每個區域在15分鐘的時間範圍內的要求數目。

```SQL
SELECT count(*) as NumberOfRequests, RegionsRefDataInput.RegionName 
FROM UserRequestStreamDataInput
JOIN RegionsRefDataInput 
ON st_within(UserRequestStreamDataInput.FromLocation, RegionsRefDataInput.Geofence) = 1
GROUP BY RegionsRefDataInput.RegionName, hoppingwindow(minute, 1, 15)
```

此查詢會每分鐘在城市內的每個區域輸出一次要求計數。 Power BI 儀表板可以輕鬆地顯示這項資訊，或者可以透過與 Azure 函式等服務整合，將所有驅動程式廣播到所有驅動程式作為 SMS 文字訊息。

下圖說明 Power BI 儀表板的查詢輸出。 

![Power BI 儀表板上的結果輸出](./media/geospatial-scenarios/power-bi-output.png)


## <a name="next-steps"></a>後續步驟

* [串流分析地理空間函式簡介](stream-analytics-geospatial-functions.md)
* [地理空間函數（Azure 串流分析）](https://docs.microsoft.com/stream-analytics-query/geospatial-functions)
