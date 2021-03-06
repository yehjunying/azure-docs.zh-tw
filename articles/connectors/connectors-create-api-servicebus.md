---
title: 與 Azure 服務匯流排交換訊息
description: 建立自動化的工作和工作流程，以使用中的 Azure 服務匯流排來傳送和接收訊息 Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 09/19/2019
tags: connectors
ms.openlocfilehash: 1b38b8508dbe17d42bf191149410f5db638cf834
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "76261614"
---
# <a name="exchange-messages-in-the-cloud-by-using-azure-logic-apps-and-azure-service-bus"></a>使用 Azure Logic Apps 和 Azure 服務匯流排在雲端中交換訊息

透過[Azure Logic Apps](../logic-apps/logic-apps-overview.md)和[Azure 服務匯流排](../service-bus-messaging/service-bus-messaging-overview.md)連接器，您可以建立自動化的工作和工作流程，以傳送資料給組織的應用程式，例如銷售和採購訂單、日誌和庫存移動。 連接器不只會監視、傳送及管理訊息，也會執行佇列、工作階段、主題、訂用帳戶等等的動作，例如：

* 監視訊息何時在佇列、主題及主題訂用帳戶中送達 (自動完成) 或接收 (查看鎖定)。
* 傳送訊息。
* 建立和刪除主題訂用帳戶。
* 管理佇列和主題訂用帳戶中的訊息，例如，取得、延後、完成、延遲、放棄和無效信件。
* 更新佇列和主題訂用帳戶中訊息和工作階段的鎖定。
* 關閉佇列和主題中的工作階段。

您可以使用觸發程序，從服務匯流排收到回應，並且讓輸出可供邏輯應用程式中的其他動作使用。 您也可以讓其他動作使用服務匯流排動作的輸出。 如果您不熟悉服務匯流排和 Logic Apps，請參閱[什麼是 Azure 服務匯流排？](../service-bus-messaging/service-bus-messaging-overview.md)和[Azure Logic Apps](../logic-apps/logic-apps-overview.md)？

[!INCLUDE [Warning about creating infinite loops](../../includes/connectors-infinite-loops.md)]

## <a name="prerequisites"></a>Prerequisites

* Azure 訂用帳戶。 如果您沒有 Azure 訂用帳戶，請先[註冊免費的 Azure 帳戶](https://azure.microsoft.com/free/)。

* 服務匯流排命名空間和傳訊實體，例如佇列。 這些專案和您的邏輯應用程式都必須使用相同的 Azure 訂用帳戶。 如果您沒有這些項目，請了解如何[建立您的服務匯流排命名空間和佇列](../service-bus-messaging/service-bus-create-namespace-portal.md)。

* [如何建立邏輯應用程式的](../logic-apps/quickstart-create-first-logic-app-workflow.md)基本知識

* 您使用服務匯流排命名空間和訊息實體的邏輯應用程式。 您的邏輯應用程式和服務匯流排必須使用相同的 Azure 訂用帳戶。 若要使用服務匯流排觸發程式啟動您的工作流程，請[建立空白邏輯應用程式](../logic-apps/quickstart-create-first-logic-app-workflow.md)。 若要在您的工作流程中使用服務匯流排動作，請使用另一個觸發程式來啟動邏輯應用程式，例如「[週期」觸發](../connectors/connectors-native-recurrence.md)程式。

<a name="permissions-connection-string"></a>

## <a name="check-permissions"></a>檢查權限

確認您的邏輯應用程式有權可以存取您的服務匯流排命名空間。

1. 登入 [Azure 入口網站](https://portal.azure.com)。

1. 移至您的服務匯流排「命名空間」**。 在命名空間頁面的 [設定]**** 之下，選取 [共用存取原則]****。 在 [**宣告**] 底下，確認您有該命名空間的 [**管理**] 許可權。

   ![管理服務匯流排命名空間的許可權](./media/connectors-create-api-azure-service-bus/azure-service-bus-namespace.png)

1. 取得服務匯流排命名空間的連接字串。 當您在邏輯應用程式中提供連接資訊時，需要此字串。

   1. 在 [**共用存取原則**] 窗格上，選取 [ **RootManageSharedAccessKey**]。
   
   1. 在您的主要連接字串旁，選取 [複製] 按鈕。 儲存連接字串以供稍後使用。

      ![複製服務匯流排命名空間連接字串](./media/connectors-create-api-azure-service-bus/find-service-bus-connection-string.png)

   > [!TIP]
   > 若要確認您的連接字串，是否與您的服務匯流排命名空間或傳訊實體 (例如佇列) 相關聯，請搜尋連接字串中是否有 `EntityPath`  參數。 如果您發現這個參數，表示此連接字串適用於特定實體，但不是可用於您邏輯應用程式的正確字串。

## <a name="add-service-bus-trigger"></a>新增服務匯流排觸發程式

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. 登入[Azure 入口網站](https://portal.azure.com)，然後在邏輯應用程式設計工具中開啟空白邏輯應用程式。

1. 在搜尋方塊中，輸入「azure 服務匯流排」作為篩選準則。 從 [觸發程式] 清單中，選取您想要的觸發程式。

   例如，若要在新專案傳送至服務匯流排佇列時觸發邏輯應用程式，請選取 [**在佇列中收到訊息時（自動完成）** ] 觸發程式。

   ![選取服務匯流排觸發程序](./media/connectors-create-api-azure-service-bus/select-service-bus-trigger.png)

   所有服務匯流排觸發程式都是*長時間輪詢*觸發程式。 此描述表示觸發程式引發時，觸發程式會處理所有訊息，然後等候30秒讓佇列或主題訂用帳戶中出現更多訊息。 如果在 30 秒內未出現任何訊息，就會略過觸發程序執行。 如果有，觸發程序會繼續讀取訊息，直到佇列或主題訂閱是空的。 下一次的觸發程序輪詢，會根據觸發程序的屬性指定的循環間隔。

   有些觸發程式（例如**當一或多個訊息抵達佇列（自動完成）觸發程式時）** 可能會傳回一或多個訊息。 當這些觸發程式引發時，它們會在其中一個和由觸發程式的 [**最大訊息計數**] 屬性所指定的訊息數目之間傳回。

1. 如果您是第一次連線到您的服務匯流排命名空間，當邏輯應用程式設計工具提示您輸入連接資訊時，請遵循下列步驟。

   1. 請為您的連線提供名稱，然後選取您的服務匯流排命名空間。

      ![建立服務匯流排連線 (第一部分)](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-trigger-1.png)

      若要改為手動輸入連接字串，請選取 [**手動輸入連接資訊**]。 如果您還沒有連接字串，請了解[如何尋找您的連接字串](#permissions-connection-string)。

   1. 選取您的服務匯流排原則，然後選取 [**建立**]。

      ![建立服務匯流排連線 (第二部分)](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-trigger-2.png)

   1. 選取您想要的訊息實體，例如佇列或主題。 針對此範例，請選取您的服務匯流排佇列。
   
      ![選取服務匯流排佇列](./media/connectors-create-api-azure-service-bus/service-bus-select-queue-trigger.png)

1. 為您選取的觸發程式提供必要的資訊。 若要將其他可用的屬性新增至動作，請開啟 [**加入新的參數**] 清單，然後選取您想要的屬性。

   針對此範例的觸發程式，請選取用於檢查佇列的輪詢間隔和頻率。

   ![設定輪詢間隔](./media/connectors-create-api-azure-service-bus/service-bus-trigger-details.png)

   如需可用觸發程式和屬性的詳細資訊，請參閱連接器的[參考頁面](/connectors/servicebus/)。

1. 藉由新增您想要的動作，繼續建立邏輯應用程式。

   例如，您可以新增在新訊息送達時，會傳送電子郵件的動作。 當觸發程序檢查您的佇列並且發現新訊息時，您的邏輯應用程式會針對找到的訊息，執行已選取的動作。

## <a name="add-service-bus-action"></a>新增服務匯流排動作

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. 登入[Azure 入口網站](https://portal.azure.com)，然後在邏輯應用程式設計工具中開啟邏輯應用程式。

1. 在您要新增動作的步驟底下，選取 [**新增步驟**]。

   或者，若要在步驟之間新增動作，請將指標移到這些步驟之間的箭號上。 選取顯示的加號（**+**），然後選取 [**新增動作**]。

1. 在 [**選擇動作**] 下的 [搜尋] 方塊中，輸入「azure 服務匯流排」作為篩選準則。 從 [動作] 清單中，選取您想要的動作。 

   在此範例中，請選取 [**傳送訊息**] 動作。

   ![選取服務匯流排動作](./media/connectors-create-api-azure-service-bus/select-service-bus-send-message-action.png) 

1. 如果您的動作是第一次連接到您的服務匯流排命名空間，當邏輯應用程式設計工具提示您輸入連線資訊時，請遵循下列步驟。

   1. 請為您的連線提供名稱，然後選取您的服務匯流排命名空間。

      ![建立服務匯流排連線 (第一部分)](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-action-1.png)

      若要改為手動輸入連接字串，請選取 [**手動輸入連接資訊**]。 如果您還沒有連接字串，請了解[如何尋找您的連接字串](#permissions-connection-string)。

   1. 選取您的服務匯流排原則，然後選取 [**建立**]。

      ![建立服務匯流排連線 (第二部分)](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-action-2.png)

   1. 選取您想要的訊息實體，例如佇列或主題。 針對此範例，請選取您的服務匯流排佇列。

      ![選取服務匯流排佇列](./media/connectors-create-api-azure-service-bus/service-bus-select-queue-action.png)

1. 為您選取的動作提供必要的詳細資料。 若要將其他可用的屬性新增至動作，請開啟 [**加入新的參數**] 清單，然後選取您想要的屬性。

   例如，選取 [**內容**] 和 [**內容類型**] 屬性，以便將它們新增至動作。 然後，指定您要傳送之訊息的內容。

   ![提供訊息內容和詳細資料](./media/connectors-create-api-azure-service-bus/service-bus-send-message-details.png)

   如需可用動作及其屬性的詳細資訊，請參閱連接器的[參考頁面](/connectors/servicebus/)。

1. 藉由新增您想要的任何其他動作，繼續建立邏輯應用程式。

   例如，您可以新增一個動作來傳送電子郵件，以確認您的郵件已送出。

1. 儲存您的邏輯應用程式。 在設計工具的工具列上，選取 [儲存]****。

## <a name="connector-reference"></a>連接器參考

服務匯流排連接器可以從服務匯流排一次最多省下1500個唯一會話至連接器快取。 如果會話計數超過此限制，則會從快取中移除舊的會話。 如需詳細資訊，請參閱[訊息會話](../service-bus-messaging/message-sessions.md)。

如需觸發程式、動作和限制的其他技術詳細資料（由連接器的 OpenAPI （先前稱為 Swagger）描述所描述），請參閱連接器的[參考頁面](/connectors/servicebus/)。 如需 Azure 服務匯流排訊息的詳細資訊，請參閱[什麼是 Azure 服務匯流排](../service-bus-messaging/service-bus-messaging-overview.md)？

## <a name="next-steps"></a>後續步驟

* 了解其他 [Logic Apps 連接器](../connectors/apis-list.md)
