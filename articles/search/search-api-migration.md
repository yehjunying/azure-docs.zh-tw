---
title: 升級 REST API 版本
titleSuffix: Azure Cognitive Search
description: 審查 API 版本的差異，並瞭解將現有的程式碼遷移至最新的 Azure 認知搜尋服務 REST API 版本所需的動作。
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: edb45eebc2c4eacc2f30d13988943f097a7190fa
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "74112163"
---
# <a name="upgrade-to-the-latest-azure-cognitive-search-service-rest-api-version"></a>升級至最新的 Azure 認知搜尋服務 REST API 版本

如果您使用舊版的[搜尋 REST API](https://docs.microsoft.com/rest/api/searchservice/)，本文將協助您升級應用程式，以使用最新正式推出的 API 版本2019-05-06。

2019-05-06 版的 REST API 包含較舊版本的一些變更。 這些是大部分具有回溯相容性，因此變更程式碼應該只需要最少的工作 (視您之前使用的版本而定)。 [升級步驟](#UpgradeSteps)概述使用新功能所需的程式碼變更。

> [!NOTE]
> Azure 認知搜尋服務實例支援一系列的 REST API 版本，包括較舊的版本。 您可以繼續使用這些 API 版本，但建議您將程式碼遷移至最新版本，以便存取新的功能。

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2019-05-06"></a>2019-05-06 版的新功能
2019-05-06 版是 REST API 的最新正式推出版本。 此 API 版本中已轉換為正式運作狀態的功能包括：

* [自動完成](index-add-suggesters.md)是一項自動提示功能，可完成部分指定的詞彙輸入。

* [複雜類型](search-howto-complex-data-types.md)會在搜尋索引中提供結構化物件資料的原生支援。

* [JsonLines 剖析模式](search-howto-index-json-blobs.md)是 Azure Blob 索引的一部分，會針對每個以分行符號分隔的 JSON 實體建立一個搜尋檔。

* [Ai 擴充](cognitive-search-concept-intro.md)提供利用認知服務之 AI 擴充引擎的編制索引。

數個預覽功能版本與此正式推出的更新一致。 若要查看新預覽功能的清單，請參閱[搜尋 REST api-版本 2019-05-06-preview](search-api-preview.md)。

## <a name="breaking-changes"></a>重大變更

包含下列功能的現有程式碼將會在 api 版本 = 2019-05-06 上中斷。

### <a name="indexer-for-azure-cosmos-db---datasource-is-now-type-cosmosdb"></a>Azure Cosmos DB 資料來源的索引子現在是 "type"： "cosmosdb"

如果您使用[Cosmos DB 索引子](search-howto-index-cosmosdb.md )，您必須將變更`"type": "documentdb"`為`"type": "cosmosdb"`。

### <a name="indexer-execution-result-errors-no-longer-have-status"></a>索引子執行結果錯誤不再具有狀態

索引子執行的錯誤結構先前具有`status`元素。 已移除此元素，因為它並未提供有用的資訊。

### <a name="indexer-data-source-api-no-longer-returns-connection-strings"></a>索引子資料來源 API 不再傳回連接字串

從 API 版本2019-05-06 和 2019-05-06-Preview 開始，資料來源 API 不會再于任何 REST 作業的回應中傳回連接字串。 在先前的 API 版本中，針對使用 POST 所建立的資料來源，Azure 認知搜尋會傳回**201** ，後面接著 OData 回應，其中包含純文字格式的連接字串。

### <a name="named-entity-recognition-cognitive-skill-is-now-discontinued"></a>命名實體辨識認知技能現已中止

如果您在程式碼中呼叫[名稱實體](cognitive-search-skill-named-entity-recognition.md)辨識技能，呼叫將會失敗。 取代功能是[實體](cognitive-search-skill-entity-recognition.md)辨識。 您應該可以取代技能參考，而不需要其他變更。 這兩個版本的 API 簽章都相同。 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>升級步驟
如果您要從先前的 GA 版本（2017-11-11 或2016-09-01）升級，您可能不需要對程式碼進行任何變更，而是變更版本號碼。 您可能需要變更程式碼的唯一情況是︰

* 您的程式碼在 API 回應中傳回無法辨認的屬性時失敗。 您的應用程式預設應該會略過不了解的屬性。

* 您的程式碼仍然存在 API 要求，並嘗試將它們重新傳送給新的 API 版本。 例如，發生原因可能是您的應用程式持續保存搜尋服務 API 所傳回的接續 Token (如需詳細資訊，請在[搜尋服務 API 參考](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)) 中查找 `@search.nextPageParameters`。

如果下列其中一種情況適用您的情況，您可能需要據此變更程式碼。 否則，除非您想要開始使用版本2019-05-06 的[新功能](#WhatsNew)，否則不需要進行任何變更。

如果您是從預覽 API 版本升級，則也會套用上述內容，但您也必須注意，版本2019-05-06 中沒有可用的預覽功能：

* ["More like this" 查詢](search-more-like-this.md)
* [CSV Blob 索引](search-howto-index-csv-blobs.md)
* [Cosmos DB 索引子的 MongoDB API 支援](search-howto-index-cosmosdb.md)

如果您的程式碼使用這些功能，您將無法升級至 API 版本2019-05-06，而不需移除您的使用方式。

> [!IMPORTANT]
> 預覽版 API 是針對測試與評估，不應該用於生產環境。
> 

### <a name="upgrading-complex-types"></a>升級複雜類型

如果您的程式碼使用較舊預覽 API 版本 2017-11-11-Preview 或 2016-09-01-Preview 的複雜類型，版本2019-05-06 中有一些新的和已變更的限制，您需要注意：

+ 子欄位深度的限制，以及每個索引的複雜集合數目已減少。 如果您使用預覽 api 版本建立了超過這些限制的索引，任何嘗試使用 API 版本2019-05-06 來更新或重新建立的應用程式都會失敗。 如果這適用于您，您將需要重新設計您的架構，以符合新的限制，然後重建您的索引。

+ 在 api 版本2019-05-06 中，每份檔的複雜集合元素數目都有新的限制。 如果您使用預覽 api 版本來建立包含超過這些限制之檔的索引，任何使用 api 版本2019-05-06 重新編制該資料索引的嘗試都會失敗。 如果這適用于您，則在重新索引資料之前，您必須減少每份檔的複雜集合元素數目。

如需詳細資訊，請參閱[Azure 認知搜尋的服務限制](search-limits-quotas-capacity.md)。

### <a name="how-to-upgrade-an-old-complex-type-structure"></a>如何升級舊的複雜型別結構

如果您的程式碼使用複雜類型搭配其中一個較舊的預覽 API 版本，您可能會使用如下所示的索引定義格式：

```json
{
  "name": "hotels",  
  "fields": [
    { "name": "HotelId", "type": "Edm.String", "key": true, "filterable": true },
    { "name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": true, "facetable": false },
    { "name": "Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.microsoft" },
    { "name": "Description_fr", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.microsoft" },
    { "name": "Category", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "sortable": false, "facetable": true, "analyzer": "tagsAnalyzer" },
    { "name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": true, "sortable": true, "facetable": true },
    { "name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": true, "sortable": true, "facetable": true },
    { "name": "Rating", "type": "Edm.Double", "filterable": true, "sortable": true, "facetable": true },
    { "name": "Address", "type": "Edm.ComplexType" },
    { "name": "Address/StreetAddress", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "searchable": true },
    { "name": "Address/City", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Address/StateProvince", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Address/PostalCode", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Address/Country", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true },
    { "name": "Location", "type": "Edm.GeographyPoint", "filterable": true, "sortable": true },
    { "name": "Rooms", "type": "Collection(Edm.ComplexType)" }, 
    { "name": "Rooms/Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.lucene" },
    { "name": "Rooms/Description_fr", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene" },
    { "name": "Rooms/Type", "type": "Edm.String", "searchable": true },
    { "name": "Rooms/BaseRate", "type": "Edm.Double", "filterable": true, "facetable": true },
    { "name": "Rooms/BedOptions", "type": "Edm.String", "searchable": true },
    { "name": "Rooms/SleepsCount", "type": "Edm.Int32", "filterable": true, "facetable": true },
    { "name": "Rooms/SmokingAllowed", "type": "Edm.Boolean", "filterable": true, "facetable": true },
    { "name": "Rooms/Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "facetable": true, "analyzer": "tagsAnalyzer" }
  ]
}  
```

API 版本 2017-11-11-Preview 中引進了較新的類似樹狀結構來定義索引欄位。 在新的格式中，每個複雜欄位都有一個 fields 集合，其子欄位已定義于其中。 在 API 版本2019-05-06 中，會以獨佔方式使用這種新格式，並嘗試使用舊格式來建立或更新索引將會失敗。 如果您有使用舊格式建立的索引，則必須使用 API 版本 2017-11-11-Preview 將它們更新為新格式，才能使用 API 版本2019-05-06 進行管理。

您可以使用 API 版本 2017-11-11-Preview，透過下列步驟將「一般」索引更新為新的格式：

1. 執行 GET 要求來取得您的索引。 如果已經是新格式，就大功告成了。

2. 將索引從「平面」格式轉譯為新的格式。 您必須撰寫此程式碼，因為在撰寫本文時，沒有可用的範例程式碼。

3. 執行 PUT 要求，將索引更新為新的格式。 請務必不要變更索引的任何其他詳細資料，例如欄位的搜尋能力/filterability，因為更新索引 API 不允許這種情況。

> [!NOTE]
> 您無法從 Azure 入口網站管理以舊的「平面」格式建立的索引。 請在最早的便利性中，將您的索引從「一般」標記法升級為「樹狀結構」標記法。

## <a name="next-steps"></a>後續步驟

請參閱搜尋 REST API 參考檔。 如果您遇到問題，請向我們尋求[StackOverflow](https://stackoverflow.com/)或[連絡人支援](https://azure.microsoft.com/support/community/?product=search)的協助。

> [!div class="nextstepaction"]
> [搜尋服務 REST API 參考](https://docs.microsoft.com/rest/api/searchservice/)

