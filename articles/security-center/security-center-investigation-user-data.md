---
title: 管理 Azure 資訊安全中心調查中找到的使用者資料
description: " 了解如何管理 Azure 資訊安全中心調查功能中的使用者資料。 "
services: operations-management-suite
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2018
ms.author: memildin
ms.openlocfilehash: aa262b0be3902f6e143a53f8f1302156fc5aede6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "80582991"
---
# <a name="manage-user-data-found-in-an-azure-security-center-investigation"></a>管理 Azure 資訊安全中心調查中找到的使用者資料
本文提供有關如何管理 Azure 資訊安全中心調查功能中找到的使用者資料的資訊。 調查資料會儲存在[Azure 監視器記錄](../log-analytics/log-analytics-overview.md)檔中，並在資訊安全中心中公開。 管理使用者資料包含了刪除或匯出資料的能力。

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

## <a name="searching-for-and-identifying-personal-data"></a>搜尋及識別個人資料
在 Azure 入口網站中，您可以使用資訊安全中心的調查功能搜尋個人資料。 調查功能位於 [安全性警示]**** 下。

調查功能顯示 [實體]**** 索引標籤下的所有實體、使用者資訊和資料。

## <a name="securing-and-controlling-access-to-personal-information"></a>保護與控制個人資訊存取
指派為讀者、擁有者、參與者或帳戶管理員角色的資訊安全中心使用者，可以在此工具中存取客戶資料。

若要深入了解讀者、擁有者和參與者角色，請參閱 [Azure 角色型存取控制的內建角色](../role-based-access-control/built-in-roles.md)。 若要深入了解帳戶管理員角色，請參閱 [Azure 訂用帳戶管理員](../cost-management-billing/manage/add-change-subscription-administrator.md)。

## <a name="deleting-personal-data"></a>刪除個人資料
指派為擁有者、參與者或帳戶管理員角色的資訊安全中心使用者可以刪除調查資訊。

若要刪除調查，您可以提交 `DELETE` 要求給 Azure Resource Manager REST API：

```HTTP
DELETE
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/features/security/incidents/{incidentName}
```

藉由使用 `GET` 要求列出所有事件，可以找到 `incidentName` 輸入：

```HTTP
GET
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}/features/security/incidents
```

## <a name="exporting-personal-data"></a>匯出個人資料
指派為擁有者、參與者或帳戶管理員角色的資訊安全中心使用者可以匯出調查資訊。 若要匯出調查資訊，請移至 [實體]**** 索引標籤以複製和貼上相關資訊。

## <a name="next-steps"></a>後續步驟
如需有關如何管理使用者資料的詳細資訊，請參閱[管理 Azure 資訊安全中心中的使用者資料](security-center-privacy.md)。
若要深入瞭解如何刪除 Azure 監視器記錄中的私用資料，請參閱[如何匯出及刪除私人資料](../azure-monitor/platform/personal-data-mgmt.md#how-to-export-and-delete-private-data)。
