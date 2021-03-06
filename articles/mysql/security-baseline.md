---
title: 適用于適用於 MySQL 的 Azure 資料庫的 Azure 安全性基準
description: 適用于適用於 MySQL 的 Azure 資料庫的 Azure 安全性基準
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 1b9a1771ad498fa3fb9b8294adb8a6556a00863a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "82190413"
---
# <a name="azure-security-baseline-for-azure-database-for-mysql"></a>適用于適用於 MySQL 的 Azure 資料庫的 Azure 安全性基準

適用于適用於 MySQL 的 Azure 資料庫的 Azure 安全性基準包含可協助您改善部署之安全性狀態的建議。

此服務的基準取自[Azure 安全性基準測試版本 1.0](https://docs.microsoft.com/azure/security/benchmarks/overview)，其中提供有關如何在 Azure 上使用最佳作法指引來保護雲端解決方案的建議。

如需詳細資訊，請參閱[Azure 安全性基準總覽](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)。

## <a name="network-security"></a>網路安全性

*如需詳細資訊，請參閱[安全性控制：網路安全性](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security)。*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1：在您的虛擬網路上使用網路安全性群組或 Azure 防火牆來保護資源

**指導**方針：為具有私人端點的適用於 MySQL 的 Azure 資料庫設定私用連結。 Private Link 可讓您透過私人端點連線到 Azure 中的各種 PaaS 服務。 Azure 私用連結基本上會將 Azure 服務帶入您的私用虛擬網路（VNet）內。 您的虛擬網路和 MySQL 實例之間的流量會傳送到 Microsoft 骨幹網路。

或者，您可以使用虛擬網路服務端點來保護和限制對您適用於 MySQL 的 Azure 資料庫的執行的網路存取。 虛擬網路規則是一項防火牆安全性功能，可控制適用於 MySQL 的 Azure 資料庫伺服器是否接受從虛擬網路中特定子網路所傳來的通訊。

您也可以使用防火牆規則來保護您的適用於 MySQL 的 Azure 資料庫伺服器。 伺服器防火牆會防止對您資料庫伺服器的所有存取，直到您指定哪些電腦擁有許可權為止。 若要設定您的防火牆，您可以建立防火牆規則，指定可接受的 IP 位址範圍。 您可以在伺服器層級建立防火牆規則。

如何設定適用於 MySQL 的 Azure 資料庫的私人連結：https://docs.microsoft.com/azure/mysql/howto-configure-privatelink-portal

如何在適用於 MySQL 的 Azure 資料庫中建立和管理 VNet 服務端點和 VNet 規則：https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview

如何設定適用於 MySQL 的 Azure 資料庫防火牆規則：https://docs.microsoft.com/azure/mysql/howto-manage-firewall-using-portal

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2：監視並記錄 Vnet、子網和 Nic 的設定和流量

**指引**：當您的適用於 MySQL 的 Azure 資料庫實例受到私人端點的安全保護時，您可以在相同的虛擬網路中部署虛擬機器。 您可以使用網路安全性群組（NSG）來降低資料外泄的風險。 啟用 NSG 流量記錄，並將記錄傳送到儲存體帳戶以進行流量審核。 您也可以將 NSG 流量記錄傳送到 Log Analytics 工作區，並使用「分析」來提供 Azure 雲端中流量的深入解析。 流量分析的一些優點是能夠將網路活動視覺化，並識別作用點、識別安全性威脅、瞭解流量模式，以及找出網路錯誤配置。

如何設定適用於 MySQL 的 Azure 資料庫的私人連結：https://docs.microsoft.com/azure/mysql/howto-configure-privatelink-portal

如何啟用 NSG 流量記錄：https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

如何啟用和使用流量分析：https://docs.microsoft.com/azure/network-watcher/traffic-analytics

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="13-protect-critical-web-applications"></a>1.3：保護重要的 web 應用程式

**指導**方針：不適用;這項建議適用于在 Azure App Service 或計算資源上執行的 web 應用程式。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4：拒絕與已知惡意 IP 位址的通訊

**指導**方針：使用適用於 MySQL 的 Azure 資料庫的 Advanced 威脅防護。 先進的威脅防護會偵測異常活動，指出不尋常且可能有害的嘗試存取或惡意探索資料庫。

在與您適用於 MySQL 的 Azure 資料庫實例相關聯的虛擬網路上啟用 DDoS 保護標準，以防範 DDoS 攻擊。 使用 Azure 資訊安全中心整合式威脅情報來拒絕與已知惡意或未使用的網際網路 IP 位址的通訊。

如何設定適用於 MySQL 的 Azure 資料庫的先進威脅防護：https://docs.microsoft.com/azure/mysql/howto-database-threat-protection-portal

如何設定 DDoS 保護：https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="15-record-network-packets-and-flow-logs"></a>1.5：記錄網路封包和流量記錄

**指引**：當您的適用於 MySQL 的 Azure 資料庫實例受到私人端點的安全保護時，您可以在相同的虛擬網路中部署虛擬機器。 接著，您可以設定網路安全性群組（NSG），以降低資料外泄的風險。 啟用 NSG 流量記錄，並將記錄傳送到儲存體帳戶以進行流量審核。 您也可以將 NSG 流量記錄傳送到 Log Analytics 工作區，並使用「分析」來提供 Azure 雲端中流量的深入解析。 流量分析的一些優點是能夠將網路活動視覺化，並識別作用點、識別安全性威脅、瞭解流量模式，以及找出網路錯誤配置。

如何啟用 NSG 流量記錄：https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

如何啟用和使用流量分析：https://docs.microsoft.com/azure/network-watcher/traffic-analytics

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6：部署以網路為基礎的入侵偵測/入侵預防系統（IDS/IPS）

**指導**方針：使用適用於 MySQL 的 Azure 資料庫的 Advanced 威脅防護。 先進的威脅防護會偵測異常活動，指出不尋常且可能有害的嘗試存取或惡意探索資料庫。

如何設定適用於 MySQL 的 Azure 資料庫的先進威脅防護：https://docs.microsoft.com/azure/mysql/howto-database-threat-protection-portal

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="17-manage-traffic-to-web-applications"></a>1.7：管理 web 應用程式的流量

**指導**方針：不適用;這項建議適用于在 Azure App Service 或計算資源上執行的 web 應用程式。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8：將網路安全性規則的複雜性和系統管理負擔降至最低

**指導**方針：對於需要存取適用於 MySQL 的 Azure 資料庫實例的資源，請使用虛擬網路服務標籤來定義網路安全性群組或 Azure 防火牆上的網路存取控制。 建立安全性規則時，您可以使用服務標籤取代特定的 IP 位址。 藉由指定服務標記名稱（例如 SQL）。WestUs）在規則的適當 [來源] 或 [目的地] 欄位中，您可以允許或拒絕對應服務的流量。 Microsoft 會管理服務標籤所包含的位址前置詞，並隨著位址變更自動更新服務標記。

注意：適用於 MySQL 的 Azure 資料庫使用 "Microsoft .Sql" 服務標記。

如需使用服務標記的詳細資訊：https://docs.microsoft.com/azure/virtual-network/service-tags-overview

瞭解適用於 MySQL 的 Azure 資料庫的服務標記使用方式：https://docs.microsoft.com/azure/mysql/concepts-data-access-and-security-vnet#terminology-and-description

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9：維護網路裝置的標準安全性設定

**指導**方針：使用 Azure 原則定義和執行與您的適用於 MySQL 的 Azure 資料庫實例相關聯的網路設定和網路資源的標準安全性設定。 使用 "Microsoft.dbformysql" 和 "Microsoft" 命名空間中 Azure 原則別名來建立自訂原則，以審核或強制執行適用於 MySQL 的 Azure 資料庫實例的網路設定。 您也可以使用與網路或適用於 MySQL 的 Azure 資料庫實例相關的內建原則定義，例如：

- 應啟用 DDoS 保護標準

- 應為 MySQL 資料庫伺服器啟用 [強制執行 SSL 連線]

如何設定和管理 Azure 原則：https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

網路的 Azure 原則範例：https://docs.microsoft.com/azure/governance/policy/samples/

如何建立 Azure 藍圖：https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="110-document-traffic-configuration-rules"></a>1.10：檔流量設定規則

**指導**方針：使用標記來取得與適用於 MySQL 的 Azure 資料庫實例的網路安全性和流量流程相關的資源，以提供中繼資料和邏輯組織。

使用與標記相關的任何內建 Azure 原則定義，例如「需要標記和其值」，以確保所有資源都是以標籤建立，並通知您現有的未標記資源。

您可以使用 Azure PowerShell 或 Azure CLI，根據其標記來查閱或執行資源的動作。

如何建立和使用標記：https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11：使用自動化工具來監視網路資源設定並偵測變更

**指引**：使用 Azure 活動記錄來監視網路資源設定，並偵測與適用於 MySQL 的 Azure 資料庫實例相關的網路資源變更。 在 Azure 監視器中建立警示，以在重大網路資源的變更發生時觸發。

如何查看和取出 Azure 活動記錄事件：https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view

如何在 Azure 監視器中建立警示：https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="logging-and-monitoring"></a>記錄和監視

*如需詳細資訊，請參閱[安全性控制：記錄和監視](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring)。*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1：使用已核准的時間同步處理來源

**指引**： Microsoft 會維護 Azure 資源所使用的時間來源，例如記錄中時間戳記的適用於 MySQL 的 Azure 資料庫。

**Azure 資訊安全中心監視**：不適用

**責任**： Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2：設定中央安全性記錄管理

**指引**：啟用診斷設定和伺服器記錄檔，並內嵌記錄來匯總適用於 MySQL 的 Azure 資料庫實例所產生的安全性資料。 在 Azure 監視器中，使用 Log Analytics 工作區來查詢和執行分析，並將 Azure 儲存體帳戶用於長期/封存儲存體。 或者，您可以啟用和內部資料，以 Azure Sentinel 或協力廠商 SIEM。

瞭解適用於 MySQL 的 Azure 資料庫的伺服器記錄檔：https://docs.microsoft.com/azure/mysql/concepts-monitoring#server-logs

如何上架 Azure Sentinel：https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3：啟用 Azure 資源的審核記錄

**指引**：在您的適用於 MySQL 的 Azure 資料庫實例上啟用診斷設定，以存取 audit、緩慢查詢和 MySQL 計量記錄。 請確定您已特別啟用 MySQL Audit 記錄檔。 活動記錄（可自動取得）包含事件來源、日期、使用者、時間戳記、來源位址、目的地位址，以及其他實用的元素。 您也可以啟用 Azure 活動記錄診斷設定，並將記錄傳送到相同的 Log Analytics 工作區或儲存體帳戶。

瞭解適用於 MySQL 的 Azure 資料庫的伺服器記錄檔：https://docs.microsoft.com/azure/mysql/concepts-monitoring#server-logs

如何設定及存取適用於 MySQL 的 Azure 資料庫的緩慢查詢記錄：https://docs.microsoft.com/azure/mysql/howto-configure-server-logs-in-portal

如何設定及存取適用於 MySQL 的 Azure 資料庫的 audit 記錄：https://docs.microsoft.com/azure/mysql/howto-configure-audit-logs-portal

如何設定 Azure 活動記錄的診斷設定：https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4：從作業系統收集安全性記錄

**指導**方針：不適用;這項建議適用于計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="25-configure-security-log-storage-retention"></a>2.5：設定安全性記錄儲存體保留

**指引**：在 Azure 監視器中，針對用來保存適用於 MySQL 的 Azure 資料庫記錄的 Log Analytics 工作區，請根據貴組織的合規性法規來設定保留週期。 針對長期/封存儲存體使用 Azure 儲存體帳戶。

如何設定 Log Analytics 工作區的記錄檔保留參數：https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period

將資源記錄儲存在 Azure 儲存體帳戶中：https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-collect-storage

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="26-monitor-and-review-logs"></a>2.6：監視和審核記錄

**指引**：分析和監視來自適用於 MySQL 的 Azure 資料庫實例的記錄，以瞭解異常行為。 使用 Azure 監視器的 Log Analytics 來審查記錄，並對記錄資料執行查詢。 或者，您可以啟用和內部資料，以 Azure Sentinel 或協力廠商 SIEM。

如何上架 Azure Sentinel：https://docs.microsoft.com/azure/sentinel/quickstart-onboard

如需 Log Analytics 的詳細資訊：https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal

如何在 Azure 監視器中執行自訂查詢：https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7：啟用異常活動的警示

**指導**方針：啟用適用於 MySQL 的 Azure 資料庫的 [先進的威脅防護]。 先進的威脅防護會偵測異常活動，指出不尋常且可能有害的嘗試存取或惡意探索資料庫。

此外，您可以啟用 MySQL 的伺服器記錄和診斷設定，並將記錄傳送到 Log Analytics 工作區。 將您的 Log Analytics 工作區上架到 Azure Sentinel，因為它提供安全性協調流程自動化回應（攀升情況）解決方案。 這可讓您建立和使用腳本來修復安全性問題。

如何啟用適用於 MySQL 的 Azure 資料庫的先進威脅防護（預覽）：https://docs.microsoft.com/azure/mysql/howto-database-threat-protection-portal

瞭解適用於 MySQL 的 Azure 資料庫的伺服器記錄檔：https://docs.microsoft.com/azure/mysql/concepts-monitoring#server-logs

如何設定及存取適用於 MySQL 的 Azure 資料庫的緩慢查詢記錄：https://docs.microsoft.com/azure/mysql/howto-configure-server-logs-in-portal

如何設定及存取適用於 MySQL 的 Azure 資料庫的 audit 記錄：https://docs.microsoft.com/azure/mysql/howto-configure-audit-logs-portal

如何設定 Azure 活動記錄的診斷設定：https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy

如何上架 Azure Sentinel：https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="28-centralize-anti-malware-logging"></a>2.8：集中反惡意程式碼記錄

**指導**方針：不適用;適用於 MySQL 的 Azure 資料庫不會處理或產生與反惡意程式碼相關的記錄檔。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="29-enable-dns-query-logging"></a>2.9：啟用 DNS 查詢記錄

**指導**方針：不適用;適用於 MySQL 的 Azure 資料庫不會處理或產生 DNS 相關的記錄。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="210-enable-command-line-audit-logging"></a>2.10：啟用命令列審核記錄

**指導**方針：不適用;這項建議適用于計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

## <a name="identity-and-access-control"></a>身分識別與存取控制

*如需詳細資訊，請參閱[安全性控制：身分識別和存取控制](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control)。*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1：維護系統管理帳戶的清查

**指導**方針：維護具有 Azure Database for MySQLinstances 管理平面（例如 Azure 入口網站）系統管理存取權的使用者帳戶清查。 此外，請維護可存取適用於 MySQL 的 Azure 資料庫實例之資料平面（在資料庫本身內）的系統管理帳戶清查。 （建立 MySQL 伺服器時，您會提供系統管理員使用者的認證。 此系統管理員可以用來建立其他 MySQL 使用者。）

適用於 MySQL 的 Azure 資料庫不支援內建的角色型存取控制，但是您可以根據特定的資源提供者選項來建立自訂角色。

瞭解 Azure 訂用帳戶的自訂角色：https://docs.microsoft.com/azure/role-based-access-control/custom-roles 

瞭解適用於 MySQL 的 Azure 資料庫資源提供者作業：https://docs.microsoft.com/azure/role-based-access-control/resource-provider-operations#microsoftdbformysql

瞭解適用於 MySQL 的 Azure 資料庫的存取管理：https://docs.microsoft.com/azure/mysql/concepts-security#access-management

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="32-change-default-passwords-where-applicable"></a>3.2：變更預設密碼（若適用）

**指導**方針： Azure AD 沒有預設密碼的概念。

在建立適用於 MySQL 的 Azure 資料庫資源本身時，Azure 會強制建立具有強式密碼的系統管理使用者。 不過，建立 MySQL 實例之後，您可以使用您建立的第一個伺服器系統管理員帳戶來建立其他使用者，並授與系統管理存取權。 建立這些帳戶時，請確定您為每個帳戶設定了不同的強式密碼。

如何建立適用於 MySQL 的 Azure 資料庫的其他帳戶：https://docs.microsoft.com/azure/mysql/howto-create-users

如何更新管理密碼：https://docs.microsoft.com/azure/mysql/howto-create-manage-server-portal#update-admin-password

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="33-use-dedicated-administrative-accounts"></a>3.3：使用專用的系統管理帳戶

**指導**方針：使用可存取您適用於 MySQL 的 Azure 資料庫實例的專用系統管理帳戶，建立相關的標準操作程式。 使用 Azure 資訊安全中心的身分識別與存取管理來監視系統管理帳戶的數目。

瞭解 Azure 資訊安全中心身分識別和存取：https://docs.microsoft.com/azure/security-center/security-center-identity-access

瞭解如何在適用於 MySQL 的 Azure 資料庫中建立系統管理員使用者：https://docs.microsoft.com/azure/mysql/howto-create-users

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4：搭配 Azure Active Directory 使用單一登入（SSO）

**指引**.. 登入適用於 MySQL 的 Azure 資料庫的支援方式是使用直接在資料庫中設定的使用者名稱/密碼，以及使用 AZURE ACTIVE DIRECTORY （AD）身分識別並利用 Azure AD token 來連接。 使用 Azure AD token 時，支援不同的方法，例如 Azure AD 使用者、Azure AD 群組，或連接到資料庫的 Azure AD 應用程式。

適用于 MySQL 的控制平面存取會分別透過 REST API 提供，並支援 SSO。 若要進行驗證，請將要求的授權標頭設定為您從 Azure Active Directory 取得的 JSON Web 權杖。

使用 Azure Active Directory 以適用於 MySQL 的 Azure 資料庫進行驗證：https://docs.microsoft.com/azure/mysql/howto-configure-sign-in-azure-ad-authentication

瞭解適用於 MySQL 的 Azure 資料庫 REST API：https://docs.microsoft.com/rest/api/mysql/

瞭解具有 Azure AD 的 SSO：https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5：針對所有以 Azure Active Directory 為基礎的存取使用多重要素驗證

**指引**：啟用 Azure Active Directory 多重要素驗證（MFA），並遵循 Azure 資訊安全中心身分識別和存取管理建議。 利用 Azure AD token 來登入您的資料庫時，這可讓您針對資料庫登入要求多重要素驗證。

如何在 Azure 中啟用 MFA：https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

使用 Azure Active Directory 以適用於 MySQL 的 Azure 資料庫進行驗證：https://docs.microsoft.com/azure/mysql/howto-configure-sign-in-azure-ad-authentication

如何監視 Azure 資訊安全中心內的身分識別和存取：https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6：使用專用電腦（特殊許可權存取工作站）進行所有系統管理工作

**指導**方針：使用特殊許可權存取工作站（paw）搭配設定用來登入和設定 Azure 資源的多重要素驗證（MFA）。

瞭解特殊許可權存取工作站：https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

如何在 Azure 中啟用 MFA：https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7：從系統管理客戶紀錄和警示可疑活動

**指導**方針：為適用於 MySQL 的 Azure 資料庫啟用 [先進的威脅防護]，以產生可疑活動的警示。

此外，當環境中發生可疑或不安全的活動時，您可以使用 Azure AD Privileged Identity Management （PIM）來產生記錄檔和警示。

使用 Azure AD 風險偵測來根據有風險的使用者行為來查看警示和報告。

如何設定適用於 MySQL 的 Azure 資料庫的先進威脅防護：https://docs.microsoft.com/azure/mysql/howto-database-threat-protection-portal

如何部署 Privileged Identity Management （PIM）：https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan

瞭解 Azure AD 風險偵測：https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8：僅從核准的位置管理 Azure 資源

**指導**方針：使用名為「位置」的條件式存取，以允許入口網站和 Azure Resource Manager 只能從 IP 位址範圍或國家/地區的特定邏輯群組進行存取。

如何在 Azure 中設定命名位置：https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="39-use-azure-active-directory"></a>3.9：使用 Azure Active Directory

**指導**方針：使用 AZURE ACTIVE DIRECTORY （AD）做為中央驗證和授權系統。 Azure AD 使用強式加密來保護待用資料和傳輸中的資料。 Azure AD 也會 salts、雜湊並安全地儲存使用者認證。

若要登入適用於 MySQL 的 Azure 資料庫，建議使用 Azure AD 並利用 Azure AD token 來進行連線。 使用 Azure AD token 時，支援不同的方法，例如 Azure AD 使用者、Azure AD 群組，或連接到資料庫的 Azure AD 應用程式。 

Azure AD 認證也可以用於管理平面層級（例如 Azure 入口網站）的管理，以控制 MySQL 管理員帳戶。

使用 Azure Active Directory 以適用於 MySQL 的 Azure 資料庫進行驗證：https://docs.microsoft.com/azure/mysql/howto-configure-sign-in-azure-ad-authentication

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10：定期審查並協調使用者存取

**指導**方針：請參閱 Azure Active Directory 記錄，以協助探索過時的帳戶，其中可能包含具有適用於 MySQL 的 Azure 資料庫系統管理角色的帳戶。 此外，您可以使用 Azure 身分識別存取審查來有效率地管理群組成員資格、存取可用來存取適用於 MySQL 的 Azure 資料庫的企業應用程式，以及角色指派。 應定期檢查使用者存取，例如每隔90天，以確保只有適當的使用者可以繼續存取。

瞭解 Azure AD 報告https://docs.microsoft.com/azure/active-directory/reports-monitoring/

如何使用 Azure 身分識別存取權審查：https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11：監視嘗試存取停用的帳戶

**指導**方針：啟用適用於 MySQL 的 Azure 資料庫和 Azure Active Directory 的診斷設定，並將所有記錄傳送到 Log Analytics 工作區。 在 Log Analytics 中設定所需的警示（例如失敗的驗證嘗試）。

如何設定及存取適用於 MySQL 的 Azure 資料庫的緩慢查詢記錄：https://docs.microsoft.com/Azure/mysql/howto-configure-server-logs-in-portal

如何設定及存取適用於 MySQL 的 Azure 資料庫的 audit 記錄：https://docs.microsoft.com/Azure/mysql/howto-configure-audit-logs-portal

如何將 Azure 活動記錄整合到 Azure 監視器：https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12：帳戶登入行為偏差的警示

**指導**方針：為適用於 MySQL 的 Azure 資料庫啟用 [先進的威脅防護]，以產生可疑活動的警示。

使用 Azure Active Directory 的身分識別保護和風險偵測功能，針對偵測到的可疑動作設定自動回應。 您可以透過 Azure Sentinel 啟用自動回應，以執行貴組織的安全性回應。

您也可以將記錄內嵌到 Azure Sentinel，以進行進一步的調查。

如何設定適用於 MySQL 的 Azure 資料庫的先進威脅防護：https://docs.microsoft.com/azure/mysql/howto-database-threat-protection-portal

Azure AD Identity Protection 的總覽：https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection

如何觀看 Azure AD 有風險的登入：https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins

如何上架 Azure Sentinel：https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13：提供 Microsoft 在支援案例期間存取相關的客戶資料

**指導**方針：不適用;適用於 MySQL 的 Azure 資料庫尚不支援客戶加密箱。

客戶加密箱支援的服務清單：https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

## <a name="data-protection"></a>資料保護

*如需詳細資訊，請參閱[安全性控制：資料保護](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection)。*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1：維護機密資訊的清查

**指引**：使用標記來協助追蹤適用於 MySQL 的 Azure 資料庫實例或儲存或處理敏感資訊的相關資源。

如何建立和使用標記：https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2：隔離儲存或處理敏感資訊的系統

**指引**：在開發、測試和生產環境中，執行不同的訂用帳戶和/或管理群組。 使用私人連結、服務端點和（或）防火牆規則的組合，來隔離及限制適用於 MySQL 的 Azure 資料庫實例的網路存取。

如何建立額外的 Azure 訂用帳戶：https://docs.microsoft.com/azure/billing/billing-create-subscription

如何建立管理群組：https://docs.microsoft.com/azure/governance/management-groups/create

如何設定適用於 MySQL 的 Azure 資料庫的私人連結：https://docs.microsoft.com/azure/mysql/concepts-data-access-security-private-link

如何在適用於 MySQL 的 Azure 資料庫中建立和管理 VNet 服務端點和 VNet 規則：https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview

如何設定適用於 MySQL 的 Azure 資料庫防火牆規則：https://docs.microsoft.com/azure/mysql/concepts-firewall-rules


**Azure 資訊安全中心監視**：無法使用

**責任**：客戶

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3：監視並封鎖未經授權的機密資訊傳輸

**指引**：使用 Azure vm 來存取適用於 MySQL 的 Azure 資料庫實例時，請使用私人連結、MySQL 網路設定、網路安全性群組和服務標籤，以減輕資料外泄的可能性。

Microsoft 會管理適用於 MySQL 的 Azure 資料庫的基礎結構，並已實行嚴格的控制，以避免遺失或公開客戶資料。

如何緩和適用於 MySQL 的 Azure 資料庫的資料外泄：https://docs.microsoft.com/azure/mysql/concepts-data-access-security-private-link#data-exfiltration-prevention

瞭解 Azure 中的客戶資料保護：https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4：加密傳輸中的所有機密資訊

**指導**方針：適用於 MySQL 的 Azure 資料庫支援使用安全通訊端層（SSL）將 MySQL 伺服器連接到用戶端應用程式。 在您的資料庫伺服器和用戶端應用程式之間強制使用 SSL 連線，可將兩者之間的資料流加密，有助於抵禦「中間人」攻擊。 在 Azure 入口網站中，根據預設，請確定您的所有適用於 MySQL 的 Azure 資料庫實例都已啟用 [強制執行 SSL 連線]。

適用於 MySQL 的 Azure 資料庫目前支援的 TLS 版本為 TLS 1.0、TLS 1.1、TLS 1.2。

如何在傳輸中設定適用於 MySQL 的 Azure 資料庫的加密：https://docs.microsoft.com/azure/mysql/concepts-ssl-connection-security

**Azure 資訊安全中心監視**：無法使用

**責任**：共用

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5：使用 active discovery 工具來識別敏感性資料

**指導**方針：適用於 MySQL 的 Azure 資料庫尚無法使用資料識別、分類和遺失防護功能。 視需要執行協力廠商解決方案，以符合合規性目的。

針對由 Microsoft 管理的基礎平臺，Microsoft 會將所有客戶內容視為機密，並移至絕佳的長度，以防範客戶資料遺失和暴露。 為了確保 Azure 中的客戶資料保持安全，Microsoft 已實行並維護一套強大的資料保護控制和功能。

瞭解 Azure 中的客戶資料保護：https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure 資訊安全中心監視**：無法使用

**責任**：共用

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6：使用 Azure RBAC 來控制資源的存取權

**指引**：使用 Azure 角色型存取控制（RBAC）來控制適用於 MySQL 的 Azure 資料庫控制平面（例如 Azure 入口網站）的存取權。 對於資料平面存取（在資料庫本身），請使用 SQL 查詢來建立使用者並設定使用者權限。 RBAC 不會影響資料庫內的使用者權限。

如何在 Azure 中設定 RBAC：https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal

如何使用 SQL 為適用於 MySQL 的 Azure 資料庫設定使用者存取權：https://docs.microsoft.com/azure/mysql/howto-create-users

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7：使用以主機為基礎的資料遺失防護來強制存取控制

**指導**方針：不適用;這項建議適用于計算資源。

Microsoft 會管理適用於 MySQL 的 Azure 資料庫的基礎結構，並已實行嚴格的控制，以避免遺失或公開客戶資料。

瞭解 Azure 中的客戶資料保護：https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure 資訊安全中心監視**：不適用

**責任**： Microsoft

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8：加密機密資訊待用

**指導**方針：適用於 MySQL 的 Azure 資料庫服務會使用 FIPS 140-2 驗證的密碼編譯模組，以進行待用資料的儲存體加密。 包含備份在內的資料會在磁片上進行加密，但執行查詢時所建立的暫存檔案除外。 該服務使用包含在 Azure 儲存體加密中的 AES 256 位元加密，且金鑰是由系統進行管理。 儲存體加密會一律啟用，且無法停用。

適用於 MySQL 的 Azure 資料庫使用客戶管理的金鑰進行資料加密，可讓您攜帶自己的金鑰（BYOK）來保護待用資料。 此時，您必須要求存取權才能使用這項功能。 若要這麼做，請聯絡：

AskAzureDBforMySQL@service.microsoft.com

瞭解適用於 MySQL 的 Azure 資料庫的待用加密：https://docs.microsoft.com/azure/mysql/concepts-security

如何設定適用於 MySQL 的 Azure 資料庫的客戶管理金鑰：https://docs.microsoft.com/azure/mysql/concepts-data-encryption-mysql

**Azure 資訊安全中心監視**：不適用

**責任**： Microsoft

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9：對重要 Azure 資源的變更進行記錄和警示

**指導**方針：使用 Azure 監視器搭配 Azure 活動記錄，以針對適用於 MySQL 的 Azure 資料庫的生產實例和其他重要或相關資源進行變更時，建立警示。

如何建立 Azure 活動記錄事件的警示：https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="vulnerability-management"></a>弱點管理

*如需詳細資訊，請參閱[安全性控制：弱點管理](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)。*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1：執行自動化弱點掃描工具

**指導**方針：目前無法使用;Azure 資訊安全中心尚未支援適用於 MySQL 的 Azure 資料庫的弱點評估。

Azure 資訊安全中心中 Azure PaaS 服務的功能涵蓋範圍：https://docs.microsoft.com/azure/security-center/features-paas

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2：部署自動化的作業系統修補程式管理解決方案

**指導**方針：不適用;此指導方針適用于計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3：部署自動化的協力廠商軟體修補程式管理解決方案

**指導**方針：不適用;此指導方針適用于計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4：比較回溯弱點掃描

**指導**方針：不適用;此指導方針適用于計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5：使用風險評等程式來排定所發現弱點的補救優先順序

**指導**方針： Microsoft 會在支援適用於 MySQL 的 Azure 資料庫的基礎系統上執行弱點管理。


**Azure 資訊安全中心監視**：不適用

**責任**： Microsoft

## <a name="inventory-and-asset-management"></a>清查和資產管理

*如需詳細資訊，請參閱[安全性控制：清查和資產管理](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management)。*

### <a name="61-use-azure-asset-discovery"></a>6.1：使用 Azure 資產探索

**指引**：使用 Azure Resource Graph 來查詢和探索訂用帳戶內的所有資源（包括適用於 MySQL 的 Azure 資料庫實例）。 請確定您的租使用者中有適當的（讀取）許可權，而且能夠列舉所有的 Azure 訂用帳戶以及您訂用帳戶內的資源。

如何使用 Azure Resource Graph 建立查詢：https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

如何查看您的 Azure 訂用帳戶：https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

瞭解 Azure RBAC：https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="62-maintain-asset-metadata"></a>6.2：維護資產中繼資料

**指引**：將標籤套用至適用於 MySQL 的 Azure 資料庫實例和其他相關資源，以邏輯方式將它們組織成分類法。

如何建立和使用標記：https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="63-delete-unauthorized-azure-resources"></a>6.3：刪除未經授權的 Azure 資源

**指引**：適當地使用標記、管理群組和個別訂用帳戶來組織和追蹤適用於 MySQL 的 Azure 資料庫實例和相關資源。 定期協調清查，並確保未經授權的資源會及時從訂用帳戶中刪除。

如何建立額外的 Azure 訂用帳戶：https://docs.microsoft.com/azure/billing/billing-create-subscription

如何建立管理群組：https://docs.microsoft.com/azure/governance/management-groups/create

如何建立和使用標記：https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6.4：維護已核准 Azure 資源和軟體標題的清查

**指導**方針：不適用;這項建議適用于計算資源和 Azure 整體。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5：未核准的 Azure 資源的監視

**指引**：使用 Azure 原則來對可使用下列內建原則定義在客戶訂用帳戶中建立的資源類型進行限制：

- 不允許的資源類型

- 允許的資源類型

此外，請使用 Azure Resource Graph 來查詢/探索訂用帳戶內的資源。

如何設定和管理 Azure 原則：https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

如何使用 Azure Graph 建立查詢：https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6：監視計算資源內未經核准的軟體應用程式

**指導**方針：不適用;這項建議適用于計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7：移除未核准的 Azure 資源和軟體應用程式

**指導**方針：不適用;這項建議適用于計算資源和 Azure 整體。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="68-use-only-approved-applications"></a>6.8：僅使用已核准的應用程式

**指導**方針：不適用;這項建議適用于計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="69-use-only-approved-azure-services"></a>6.9：僅使用已核准的 Azure 服務

**指引**：使用 Azure 原則來對可使用下列內建原則定義在客戶訂用帳戶中建立的資源類型進行限制：

- 不允許的資源類型

- 允許的資源類型

如何設定和管理 Azure 原則：https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

如何拒絕具有 Azure 原則的特定資源類型：https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="610-implement-approved-application-list"></a>6.10：執行核准的應用程式清單

**指導**方針：不適用;這項建議適用于計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="611-limit-users-ability-to-interact-with-azure-resources-manager-via-scripts"></a>6.11：限制使用者透過腳本與 Azure 資源管理員互動的能力

**指引**：使用 Azure 條件式存取來限制使用者與 Azure Resource Manager 的互動能力，方法是設定「Microsoft Azure 管理」應用程式的「封鎖存取」。 這可能會防止在高安全性環境（例如包含機密資訊的適用於 MySQL 的 Azure 資料庫實例）中建立和變更資源。

如何設定條件式存取以封鎖對 Azure Resource Manager 的存取：https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12：限制使用者在計算資源內執行腳本的能力

**指導**方針：不適用;這項建議適用于計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13：實際或邏輯上隔離高風險應用程式

**指導**方針：不適用;這項建議適用于在 Azure App Service 或計算資源上執行的 web 應用程式。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

## <a name="secure-configuration"></a>安全設定

*如需詳細資訊，請參閱[安全性控制：安全](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration)設定。*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1：為所有 Azure 資源建立安全設定

**指引**：使用 Azure 原則為您的適用於 MySQL 的 Azure 資料庫實例定義和執行標準安全性設定。 使用 "Microsoft.dbformysql" 命名空間中 Azure 原則別名來建立自訂原則，以審核或強制執行適用於 MySQL 的 Azure 資料庫實例的網路設定。 您也可以使用與適用於 MySQL 的 Azure 資料庫實例相關的內建原則定義，例如：

應為 MySQL 資料庫伺服器啟用 [強制執行 SSL 連線]

如何查看可用的 Azure 原則別名：https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

如何設定和管理 Azure 原則：https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="72-establish-secure-operating-system-configurations"></a>7.2：建立安全的作業系統設定

**指導**方針：不適用;這項建議適用于計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3：維護安全的 Azure 資源設定

**指導**方針：使用 Azure 原則 [拒絕] 和 [不存在時部署]，在您的 Azure 資源上強制執行安全設定。

如何設定和管理 Azure 原則：https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

瞭解 Azure 原則效果：https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4：維護安全的作業系統設定

**指導**方針：不適用;這項建議適用于計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5：安全地儲存 Azure 資源的設定

**指導**方針：如果您適用於 MySQL 的 Azure 資料庫實例和相關資源使用自訂 Azure 原則定義，請使用 Azure Repos 安全地儲存和管理您的程式碼。

如何將程式碼儲存在 Azure DevOps：https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure Repos 檔：https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="76-securely-store-custom-operating-system-images"></a>7.6：安全地儲存自訂的作業系統映射

**指導**方針：不適用;這項建議適用于計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="77-deploy-system-configuration-management-tools"></a>7.7：部署系統設定管理工具

**指導**方針：使用 "microsoft.dbformysql" 命名空間中的 Azure 原則別名來建立自訂原則，以警示、審查和強制執行系統組態。 此外，開發處理常式和管線來管理原則例外狀況。

如何設定和管理 Azure 原則：https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8：部署作業系統的系統建構管理工具

**指導**方針：不適用;這項建議適用于計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9：執行 Azure 服務的自動化設定監視

**指導**方針：使用 "microsoft.dbformysql" 命名空間中的 Azure 原則別名來建立自訂原則，以警示、審查和強制執行系統組態。 使用 Azure 原則 [audit]、[deny] 和 [deploy if not 存在]，自動為您的適用於 MySQL 的 Azure 資料庫實例和相關資源強制執行設定。

如何設定和管理 Azure 原則：https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10：為作業系統執行自動化設定監視

**指導**方針：不適用;這項建議適用于計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="711-manage-azure-secrets-securely"></a>7.11：安全地管理 Azure 秘密

**指引**：針對在用來存取適用於 MySQL 的 Azure 資料庫實例 Azure App Service 上執行的 Azure 虛擬機器或 web 應用程式，請搭配使用受控服務識別與 Azure Key Vault 以簡化和保護適用於 MySQL 的 Azure 資料庫秘密管理。 請確定已啟用 Key Vault 虛刪除。

如何與 Azure 受控識別整合：https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

如何建立 Key Vault：https://docs.microsoft.com/azure/key-vault/quick-create-portal

如何使用受控識別提供 Key Vault 驗證：https://docs.microsoft.com/azure/key-vault/managed-identity

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="712-manage-identities-securely-and-automatically"></a>7.12：安全且自動地管理身分識別

**指導**方針：適用於 MySQL 的 Azure 資料庫實例支援 Azure Active Directory 驗證（預覽）來存取資料庫。  建立適用於 MySQL 的 Azure 資料庫實例時，您會提供系統管理員使用者的認證。 此系統管理員可以用來建立額外的資料庫使用者。  

針對在用來存取適用於 MySQL 的 Azure 資料庫實例 Azure App Service 上執行的 Azure 虛擬機器或 web 應用程式，請使用受控服務識別搭配 Azure Key Vault 來儲存和取出適用於 MySQL 的 Azure 資料庫實例的認證。 請確定已啟用 Key Vault 虛刪除。

使用受控識別，在 Azure Active Directory （AD）中為 Azure 服務提供自動受控識別。 受控識別可讓您向任何支援 Azure AD 驗證的服務進行驗證，包括 Key Vault，而您的程式碼中沒有任何認證。

如何設定受控識別：https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

如何與 Azure 受控識別整合：https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13：消除非預期的認證暴露

**指導**方針：執行認證掃描器來識別程式碼中的認證。 認證掃描器也鼓勵將探索到的認證移至更安全的位置，例如 Azure Key Vault。

如何設定認證掃描器：https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="malware-defense"></a>惡意程式碼防禦

*如需詳細資訊，請參閱[安全性控制：惡意程式碼防護](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense)。*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1：使用集中管理的反惡意程式碼軟體

**指導**方針：不適用;這項建議適用于計算資源。

Microsoft 反惡意程式碼已在支援 Azure 服務的基礎主機上啟用（例如，適用于 SQL 的 Azure 資料庫），但不會對客戶內容執行。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2：預先掃描要上傳至非計算 Azure 資源的檔案

**指導**方針：支援 Azure 服務的基礎主機（例如適用於 MySQL 的 Azure 資料庫）已啟用 Microsoft 反惡意程式碼，但不會在客戶內容上執行。

預先掃描要上傳至非計算 Azure 資源的任何內容，例如 App Service、Data Lake Storage、Blob 儲存體、適用於 MySQL 的 Azure 資料庫等等。Microsoft 無法存取您在這些實例中的資料。

**Azure 資訊安全中心監視**：不適用

**責任**：共用

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3：確認反惡意程式碼軟體和簽章已更新

**指導**方針：不適用;這項建議適用于計算資源。

支援 Azure 服務的基礎主機（例如適用於 MySQL 的 Azure 資料庫）會啟用 Microsoft 反惡意程式碼，但不會在客戶內容上執行。

**Azure 資訊安全中心監視**：不適用

**責任**： N/A

## <a name="data-recovery"></a>資料復原

*如需詳細資訊，請參閱[安全性控制：資料](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery)復原。*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1：確保週期性自動備份

**指導**方針：適用於 MySQL 的 Azure 資料庫取得資料檔案和交易記錄檔的備份。 視支援的最大儲存體大小而定，我們會採用完整和差異備份（4 TB 的儲存體伺服器）或快照集備份（最多 16 TB 的儲存體伺服器）。 在您設定的備份保留期限內，這些備份可讓您將伺服器還原至任何時間點。 預設的備份保留期限是七天。 可選擇設定的期限最多為 35 天。 所有備份皆會使用 AES 256 位元加密進行加密。

瞭解適用於 MySQL 的 Azure 資料庫的備份：https://docs.microsoft.com/azure/mysql/concepts-backup

瞭解適用於 MySQL 的 Azure 資料庫初始設定：https://docs.microsoft.com/azure/mysql/tutorial-design-database-using-portal

**Azure 資訊安全中心監視**：是

**責任**：共用

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2：執行完整的系統備份，並備份任何客戶管理的金鑰

**指導**方針：適用於 MySQL 的 Azure 資料庫會根據使用者的選擇，自動建立伺服器備份，並將它們儲存在本機-多餘或異地多餘的儲存體中。 備份可以用來將伺服器還原至某個時間點。 備份和還原可保護資料免於意外損毀或刪除，是商務持續性策略中不可或缺的一部分。 

如果使用 Azure Key Vault 來儲存適用於 MySQL 的 Azure 資料庫實例的認證，請確保金鑰的定期自動備份。 

瞭解適用於 MySQL 的 Azure 資料庫的備份：https://docs.microsoft.com/azure/mysql/howto-restore-server-portal 

如何備份 Key Vault 金鑰：https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey


**Azure 資訊安全中心監視**：是

**責任**：共用

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3：驗證所有備份，包括客戶管理的金鑰

**指引**：在適用於 MySQL 的 Azure 資料庫中，執行還原會從源伺服器的備份建立新的伺服器。 有兩種可用的還原類型：時間點還原和異地還原。 時間點還原可搭配任一備份備援選項使用，並在您原始伺服器所在區域中建立新的伺服器。 異地還原只能您將伺服器設定為使用異地備援儲存體時使用，這可讓您將伺服器還原至不同的區域。

預估的復原時間取決於數個因素，包括資料庫大小、交易記錄大小、網路頻寬，以及在相同區域中同時進行復原的資料庫總數。 復原時間通常不到 12 小時。

定期測試適用於 MySQL 的 Azure 資料庫實例的還原。

瞭解適用於 MySQL 的 Azure 資料庫中的備份和還原：https://docs.microsoft.com/azure/mysql/concepts-backup

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4：確保備份和客戶管理金鑰的保護

**指導**方針：適用於 MySQL 的 Azure 資料庫會取得完整、差異和交易記錄備份。 在您設定的備份保留期限內，這些備份可讓您將伺服器還原至任何時間點。 預設的備份保留期限是七天。 可選擇設定的期限最多為 35 天。 所有備份皆會使用 AES 256 位元加密進行加密。 請確定已啟用 Key Vault 虛刪除。

瞭解適用於 MySQL 的 Azure 資料庫中的備份和還原：https://docs.microsoft.com/azure/mysql/concepts-backup

**Azure 資訊安全中心監視**：是

**責任**：客戶

## <a name="incident-response"></a>事件回應

*如需詳細資訊，請參閱[安全性控制：事件回應](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response)。*

### <a name="101-create-an-incident-response-guide"></a>10.1：建立事件回應指南

**指引**：為您的組織建立事件回應指南。 請確定有寫入的事件回應計畫，可定義人員的所有角色，以及從偵測到事件處理/管理的階段，以進行後續事件審查。

如何設定 Azure 資訊安全中心內的工作流程自動化：https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

建立您自己的安全性事件回應程式的指引：https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Microsoft 安全性回應中心的事件剖析：https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

客戶也可以利用 NIST 的「電腦安全性性」事件處理指南，協助建立自己的事件回應計畫：https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2：建立事件評分和優先順序程式

**指導**方針：資訊安全中心指派每個警示的嚴重性，以協助您排定應先調查哪些警示。 嚴重性是根據資訊安全中心在尋找中的信心，或用於發出警示的分析，以及導致警示的活動背後有惡意意圖的信賴等級。

此外，請清楚地標示訂閱（例如， 生產、非生產）並建立命名系統，以清楚識別和分類 Azure 資源。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="103-test-security-response-procedures"></a>10.3：測試安全性回應程式

**指導**方針：執行練習以定期測試系統的事件回應功能。 識別弱式點和間距，並視需要修訂計畫。

請參閱 NIST 的發行集：適用于 IT 計畫和功能的測試、訓練和練習程式指南：https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4：提供安全性事件連絡人詳細資料，並設定安全性事件的警示通知

**指導**方針：如果 Microsoft 安全性回應中心（MSRC）發現客戶的資料已由非法或未經授權的合作物件存取，microsoft 將會使用安全性事件連絡人資訊來與您聯繫。  檢查事實後的事件，以確保解決問題。

如何設定 Azure 資訊安全中心安全性連絡人：https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5：將安全性警示納入事件回應系統中

**指引**：使用「連續匯出」功能來匯出您的 Azure 資訊安全中心警示和建議。 「連續匯出」可讓您以手動或持續的方式來匯出警示和建議。 您可以使用 Azure 資訊安全中心資料連線器來串流「警示」 Sentinel。

如何設定連續匯出：https://docs.microsoft.com/azure/security-center/continuous-export

如何將警示串流至 Azure Sentinel：https://docs.microsoft.com/azure/sentinel/connect-azure-security-center

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="106-automate-the-response-to-security-alerts"></a>10.6：自動回應安全性警示

**指引**：使用 Azure 資訊安全中心中的工作流程自動化功能，透過「Logic Apps」安全性警示和建議，自動觸發回應。

如何設定工作流程自動化和 Logic Apps：https://docs.microsoft.com/azure/security-center/workflow-automation

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="penetration-tests-and-red-team-exercises"></a>滲透測試和 Red Team 練習

*如需詳細資訊，請參閱[安全性控制：滲透測試和 Red Team 練習](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises)。*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11.1：進行 Azure 資源的定期滲透測試，並確保在60天內補救所有重大安全性結果

**指導**方針：遵循 Microsoft Engagement 規則，確保您的滲透測試不會違反 microsoft 原則：https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1

您可以在這裡找到有關 Microsoft 管理之雲端基礎結構、服務和應用程式的 Microsoft 策略與執行的 Red 小組和即時網站滲透測試的詳細資訊：https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure 資訊安全中心監視**：不適用

**責任**：共用

## <a name="next-steps"></a>後續步驟

- 請參閱[Azure 安全性基準測試](https://docs.microsoft.com/azure/security/benchmarks/overview)
- 深入瞭解[Azure 安全性基準](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)
