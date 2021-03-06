---
title: 包含檔案
description: 包含檔案
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/24/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: de1a22ed6e9707767c0d097a9250f0bdd31414d5
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/05/2020
ms.locfileid: "82788955"
---
## <a name="create-an-image-gallery"></a>建立映像資源庫 

映像資源庫是用於啟用映像共用的主要資源。 

資源庫名稱允許的字元為大寫或小寫字母、數字、點和句點。 圖庫名稱不能包含連字號。   資源庫名稱在您的訂用帳戶內必須是唯一的。 

使用 [az sig create](/cli/azure/sig#az-sig-create) 建立映像資源庫。 下列範例會在*美國東部*建立名為*myGalleryRG*的資源群組，以及名為*myGallery*的資源庫。

```azurecli-interactive
az group create --name myGalleryRG --location eastus
az sig create --resource-group myGalleryRG --gallery-name myGallery
```

## <a name="share-the-gallery"></a>共用資源庫

您可以使用角色型存取控制（RBAC），跨訂用帳戶共用影像。 您可以在資源庫、映射定義或映射版本層級共用映射。 任何對映射版本具有讀取權限的使用者（甚至是跨訂用帳戶），都能夠使用映射版本來部署 VM。

我們建議您在資源庫層級與其他使用者共用。 若要取得資源庫的物件識別碼，請使用[az sig show](/cli/azure/sig#az-sig-show)。

```azurecli-interactive
az sig show \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --query id
```

使用物件識別碼作為範圍，以及電子郵件地址和[az role 指派 create](/cli/azure/role/assignment#az-role-assignment-create) ，以授與使用者共用映射資源庫的存取權。 將 `<email-address>` 和 `<gallery iD>` 取代為您的個人資訊。

```azurecli-interactive
az role assignment create \
   --role "Reader" \
   --assignee <email address> \
   --scope <gallery ID>
```

如需使用 RBAC 共用資源的詳細資訊，請參閱[使用 RBAC 和 Azure CLI 管理存取](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli)。
