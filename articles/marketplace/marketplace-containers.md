---
title: Azure Marketplace 中的容器供應專案發佈指南
description: 本文說明在 Azure Marketplace 中發佈容器供應專案的需求。
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: dsindona
ms.openlocfilehash: 99aecee930e5d77302ad54babd927588519e33fd
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2020
ms.locfileid: "82160455"
---
# <a name="publishing-guide-for-container-offers"></a>容器供應專案發佈指南

容器供應專案可協助您將容器映射發佈到 Azure Marketplace。 您可以使用本指南來了解這項供應項目的需求。 

容器供應專案是透過 Azure Marketplace 部署和計費的交易供應專案。 使用者會看到的呼籲行動是「立即取得」。

當您的解決方案是以 Kubernetes 為基礎的 Azure 容器服務實例所設定的 Docker 容器映射時，請使用*容器*供應專案類型。 

> [!NOTE]
> Kubernetes 型 Azure 容器服務實例的範例包括 Azure Kubernetes Service 或 Azure 容器實例，這是 Azure 客戶針對以 Kubernetes 為基礎的容器執行時間所做的選擇。  

Microsoft 目前支援免費和自備授權 (BYOL) 授權模型。

## <a name="container-offer-requirements"></a>容器供應項目需求

| 需求 | 詳細資料 |  
|:--- |:--- |  
| 計費和計量 | 支援免費或 BYOL 計費模型。<br><br> |  
| 從 Dockerfile 建立的映射 | 容器映射必須以 Docker 映射規格為基礎，並且是從 Dockerfile 建立而成。<br> <br>如需建立 Docker 映射的詳細資訊，請參閱[Dockerfile 參考](https://docs.docker.com/engine/reference/builder/#usage)的「使用方式」一節。<br><br> |  
| 在 Azure Container Registry 存放庫中裝載 | 容器映射必須裝載在 Azure Container Registry 存放庫中。<br> <br>如需使用 Azure Container Registry 的詳細資訊，請參閱[快速入門：使用 Azure 入口網站建立私人容器](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-portal)登錄。<br><br> |  
| 映像標記 | 容器映射必須包含至少一個標記（標記數目上限：16）。<br><br>如需有關標記映射的詳細資訊，請`docker tag`參閱[Docker 檔](https://docs.docker.com/engine/reference/commandline/tag)網站上的頁面。<br><br> |  

## <a name="next-steps"></a>後續步驟

如果您尚未這麼做，請瞭解如何[使用 Azure Marketplace 來拓展您的雲端業務](https://azuremarketplace.microsoft.com/sell)。

若要註冊並開始使用合作夥伴中心：

- 登[入合作夥伴中心](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership)以建立或完成您的供應專案。
- 如需詳細資訊，請參閱[建立 Azure 容器供應](./partner-center-portal/create-azure-container-offer.md)專案。
