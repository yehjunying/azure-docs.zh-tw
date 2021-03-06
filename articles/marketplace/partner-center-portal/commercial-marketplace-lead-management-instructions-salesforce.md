---
title: Salesforce 中的潛在客戶管理-Microsoft 商業 marketplace
description: 瞭解如何使用 Salesforce 來設定 Microsoft AppSource 和 Azure Marketplace 的潛在客戶
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: dsindona
ms.openlocfilehash: 7d64b8914fa0b109dfc662a97a7f84d94e3491ec
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/05/2020
ms.locfileid: "82789703"
---
# <a name="configure-lead-management-for-salesforce"></a>針對 Salesforce 設定潛在客戶管理

本文說明如何設定您的 Salesforce 系統，以在 Microsoft AppSource 和 Azure Marketplace 中處理您供應專案的銷售潛在客戶。

> [!NOTE]
> Azure Marketplace 不支援預先填入的清單，例如 [**國家/地區**] 欄位的值清單。 在繼續之前，請確定未設定任何清單。 或者，您可以設定[HTTPS 端點](./commercial-marketplace-lead-management-instructions-https.md)或[Azure 資料表](./commercial-marketplace-lead-management-instructions-azure-table.md)來接收潛在客戶。

## <a name="set-up-your-salesforce-system"></a>設定您的 Salesforce 系統

1. 登入 Salesforce。
1. 流覽至 [ **Web 到潛在客戶**] 設定。 
    
    如果您使用的是 Salesforce 光源體驗
    1. 選取 Salesforce 首頁上的 [**設定**]。

       ![Salesforce 設定](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-1.png)

    1. 在 [**設定**] 頁面上，移至 [**平臺工具** > ] [**功能** > ] [**行銷** > ] [**Web 到潛在客戶**]。

        ![Salesforce Web 到潛在客戶](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-2.png)

    如果您使用的是 Salesforce 傳統體驗：

    1. 選取 Salesforce 首頁上的 [**設定**]。

       ![Salesforce 傳統設定](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-classic-setup.png)

    1. 在 [**安裝**] 頁面上，選取 [**組建** > ] [**自訂** > **潛在** > **客戶]。**

        ![Salesforce 傳統的 Web 到潛在客戶](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-classic-web-to-lead.png)

   這兩個 Salesforce 體驗的其餘步驟都相同。

1. 在 [**網路到潛在客戶設定**] 頁面上，選取 [**建立 Web 到潛在客戶表單**] 按鈕。
1. 在 [**網路對導向設定**] 上，選取 [**建立 Web 到潛在客戶表單**]。

    ![Salesforce 網路對潛在客戶設定](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-3.png)

1. 在 [**建立 Web 到潛在客戶] 表單**上，確定`Include reCAPTCHA in HTML`已清除該設定，然後選取 [**產生**]。

    ![Salesforce 建立 Web 到潛在客戶表單窗格](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-4.png)

1. 您會看到一些 HTML 文字。 搜尋「oid」文字，並從 HTML 文字複製「 **oid」值**（僅限在引號之間的文字）並加以儲存。 您會在發佈入口網站的 [**組織識別碼**] 欄位中貼上此值。

    ![Salesforce 建立了顯示 HTML "oid" 值的 Web 到潛在客戶表單](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-5.png)

1. 選取 [完成]****。

## <a name="configure-your-offer-to-send-leads-to-salesforce"></a>設定您的供應專案以將潛在客戶傳送至 Salesforce

當您準備好在發佈入口網站中為您的供應專案設定潛在客戶管理資訊時，請遵循下列步驟。

1. 登入[合作夥伴中心](https://partner.microsoft.com/dashboard/home)。

1. 選取您的供應專案，然後移至 [**供應專案設定**] 索引標籤。

1. 在 [**潛在客戶管理**] 區段下，選取 **[連線]**。 

    ![潛在客戶管理區段 [連線] 按鈕](./media/commercial-marketplace-lead-management-instructions-salesforce/lead-management-connect.png)

1. 在 [連線**詳細資料**] 快顯視窗中，選取 [ **Salesforce** ] 作為 [**潛在客戶目的地**]，並將`oid`值從您建立的 Web 到潛在客戶表單貼入 [**組織識別碼**] 欄位。

    ![連接詳細資料快顯視窗驗證連絡人電子郵件方塊](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-connection-details.png)

1. 在 [**連絡人電子郵件**] 底下，輸入公司人員在收到新的潛在客戶時應接收電子郵件通知的電子郵件地址。 您可以提供多個電子郵件，方法是以分號分隔。

1. 選取 [確定]  。

若要確定您已成功連線到潛在客戶目的地，請選取 [**驗證**]。 如果成功，您會在潛在客戶目的地中擁有測試潛在客戶。

>[!NOTE]
>您必須先完成供應專案的其餘部分設定並加以發佈，才能接收供應專案的潛在客戶。
