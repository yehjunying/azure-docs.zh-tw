---
title: 登入自訂開發應用程式的問題 | Microsoft Docs
description: 可能導致您無法登入已開發之應用程式的常見錯誤 Azure AD
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01a424129abf88c18500c96bd1889fc6dcce2ec6
ms.sourcegitcommit: b396c674aa8f66597fa2dd6d6ed200dd7f409915
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2020
ms.locfileid: "82890709"
---
# <a name="problems-signing-in-to-a-custom-developed-application"></a>登入自訂開發應用程式的問題

有數種錯誤會導致您無法登入應用程式。 會遇到此問題的最大原因是應用程式設定錯誤。

## <a name="errors-related-to--misconfigured-apps"></a>與應用程式設定錯誤相關的錯誤

* 確認入口網站中的兩種組態符合您應用程式中的組態。 特別是，比較用戶端/應用程識別碼、回覆 URL、用戶端密碼/金鑰，及應用程式識別碼 URI。

* 將您正以程式碼要求存取的資源，與在 [必要的資源]**** 索引標籤中設定的權限比較，以確定您只要求所設定的資源。

* 請參閱 [Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory) 中任何類似的錯誤或問題。

## <a name="next-steps"></a>後續步驟

[Azure AD 開發人員指南](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[整合應用程式與 Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[Microsoft 身分識別平台端點中的權限和同意](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory>)
