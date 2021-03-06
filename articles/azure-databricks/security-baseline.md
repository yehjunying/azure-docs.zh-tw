---
title: 適用于 Azure Databricks 的 Azure 安全性基準
description: 適用于 Azure Databricks 的 Azure 安全性基準
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 7d417f412592d87dec751b9d98a26bf786c719e5
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/05/2020
ms.locfileid: "82796846"
---
# <a name="azure-security-baseline-for-azure-databricks"></a>適用于 Azure Databricks 的 Azure 安全性基準

適用于 Azure Databricks 的 Azure 安全性基準包含可協助您改善部署之安全性狀態的建議。

此服務的基準取自[Azure 安全性基準測試版本 1.0](https://docs.microsoft.com/azure/security/benchmarks/overview)，其中提供有關如何在 Azure 上使用最佳作法指引來保護雲端解決方案的建議。

如需詳細資訊，請參閱[Azure 安全性基準總覽](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)。

## <a name="network-security"></a>網路安全性

*如需詳細資訊，請參閱[安全性控制：網路安全性](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security)。*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1：在您的虛擬網路上使用網路安全性群組或 Azure 防火牆來保護資源

**指引**：在您自己的 Azure 虛擬網路（VNet）中部署 Azure Databricks。 Azure Databricks 的預設部署是 Azure 上完全受控的服務：所有資料平面資源（包括所有叢集將關聯的 VNet）都會部署至已鎖定的資源群組。 不過，如果您需要網路自訂，您可以在自己的虛擬網路（VNet 插入）中部署 Azure Databricks 資料平面資源，讓您能夠執行自訂的網路設定。 您可以使用自訂規則將自己的網路安全性群組（NSG）套用至特定的輸出流量限制。

此外，您可以設定 NSG 規則，以指定部署 Azure Databricks 實例的子網上的輸出流量限制。 Azure 防火牆可以針對 VNET 插入的工作區進行設定。

* [如何將 Azure Databricks 部署到您自己的虛擬網路](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)

* [如何管理 Nsg](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2：監視並記錄 Vnet、子網和 NIC 的設定和流量

**指引**：在您自己的 Azure 虛擬網路（VNet）中部署 Azure Databricks。 不會自動建立網路安全性群組（NSG）。 您必須使用預設的 Azure 規則來建立基準 NSG。 當您部署工作區時，會新增 Databricks 所需的規則。 您也可以從空的 NSG 開始，系統會自動新增適當的規則。 啟用 NSG 流量記錄，並將記錄傳送到儲存體帳戶以進行流量審核。 您也可以將流量記錄傳送到 Log Analytics 工作區，並使用流量分析來提供 Azure 雲端中流量的深入解析。 流量分析的一些優點是能夠將網路活動視覺化，並識別作用點、識別安全性威脅、瞭解流量模式，以及找出網路錯誤配置。

* [如何將 Azure Databricks 部署到您自己的虛擬網路](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)

* [如何啟用 NSG 流量記錄](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [如何啟用和使用流量分析](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

* [如何啟用網路監看員](https://docs.microsoft.com/azure/network-watcher/network-watcher-create)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="13-protect-critical-web-applications"></a>1.3：保護重要的 web 應用程式

**指導**方針：不適用;這項建議適用于在 Azure App Service 或計算資源上執行的 web 應用程式。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4：拒絕與已知惡意 IP 位址的通訊

**指引**：在與您的 Azure Databricks 實例相關聯的 Azure 虛擬網路上啟用 Azure DDoS 保護標準，以防範分散式阻絕服務攻擊。 使用 Azure 資訊安全中心整合式威脅情報來拒絕與已知惡意或未使用的公用 IP 位址的通訊。

* [使用 Azure 入口網站管理 Azure DDoS Protection Standard](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)

* [瞭解 Azure 網路層在 Azure 資訊安全中心中的威脅防護](https://docs.microsoft.com/azure/security-center/threat-protection#threat-protection-for-azure-network-layer-)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="15-record-network-packets-and-flow-logs"></a>1.5：記錄網路封包和流量記錄

**指引**：在您自己的 Azure 虛擬網路（VNet）中部署 Azure Databricks。 不會自動建立網路安全性群組（NSG）。 您必須使用預設的 Azure 規則來建立基準 NSG。 當您部署工作區時，會新增 Databricks 所需的規則。 啟用 NSG 流量記錄，並將記錄傳送到儲存體帳戶以進行流量審核。 您也可以將流量記錄傳送到 Log Analytics 工作區，並使用流量分析來提供 Azure 雲端中流量的深入解析。 流量分析的一些優點是能夠將網路活動視覺化，並識別作用點、識別安全性威脅、瞭解流量模式，以及找出網路錯誤配置。

* [如何將 Azure Databricks 部署到您自己的虛擬網路](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)

* [如何啟用 NSG 流量記錄](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [如何啟用和使用流量分析](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

* [如何啟用網路監看員](https://docs.microsoft.com/azure/network-watcher/network-watcher-create)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6：部署以網路為基礎的入侵偵測/入侵預防系統（IDS/IPS）

**指引**：從 Azure Marketplace 執行具備 IDP/IPS 功能的網路虛擬裝置（NVA），以建立虛擬網路整合式工作區，其中所有 Azure Databricks 叢集都有單一 IP 輸出位址。 單一 IP 位址可用來做為其他 Azure 服務和應用程式的額外安全性層級，以允許根據特定 IP 位址進行存取。 您可以使用 Azure 防火牆或其他協力廠商工具（例如 NVA）來管理輸入和輸出流量。

如果基於承載檢查的入侵偵測和/或預防不是必要條件，則可以使用具有威脅情報的 Azure 防火牆。 以 Azure 防火牆威脅情報為基礎的篩選可以警示和拒絕進出已知惡意 IP 位址和網域的流量。 IP 位址和網域來自 Microsoft 威脅情報摘要。

* [如何使用 Azure 防火牆為由 VNet 插入的工作區指派單一公用 IP](https://docs.microsoft.com/azure/databricks/kb/cloud/azure-vnet-single-ip)

* [如何建立 NVA](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal)

* [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="17-manage-traffic-to-web-applications"></a>1.7：管理 web 應用程式的流量

**指導**方針：不適用;這項建議適用于在 Azure App Service 或計算資源上執行的 web 應用程式。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8：將網路安全性規則的複雜性和系統管理負擔降至最低

**指引**：使用服務標籤來定義網路安全性群組上的網路存取控制，這些群組會附加到與您的 Azure Databricks 實例相關聯的子網。 建立安全性規則時，您可以使用服務標籤取代特定的 IP 位址。 藉由在規則的適當 [來源] 或 [目的地] 欄位中指定服務標籤名稱（例如，ApiManagement），您可以允許或拒絕對應服務的流量。 Microsoft 會管理服務標籤所包含的位址前置詞，並隨著位址變更自動更新服務標記。 非插入的 VNET 工作區不支援服務標記。

* [瞭解服務標記](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9：維護網路裝置的標準安全性設定

**指引**：使用 Azure 原則為您的 Azure Databricks 實例定義和執行網路安全性設定。 您可以使用 "Databricks" 命名空間中的 Azure 原則別名來定義自訂原則定義。 原則將不會套用至受控資源群組內的資源。

您也可以使用 Azure 藍圖，藉由在單一藍圖定義中封裝金鑰環境成品（例如 Azure 資源管理範本、角色型存取控制（RBAC）和原則），以簡化大規模的 Azure 部署。 輕鬆地將藍圖套用至新的訂用帳戶和環境，並透過版本控制微調控制和管理。

* [如何設定和管理 Azure 原則](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [瞭解 Azure 原則別名和定義結構](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure)

* [如何建立 Azure 藍圖](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="110-document-traffic-configuration-rules"></a>1.10：檔流量設定規則

**指引**.. 針對 nsg 和與 Azure Databricks 實例相關聯的網路安全性和流量流程，使用標記。 針對個別的 NSG 規則，請使用 [描述] 欄位來指定允許進出網路流量的任何規則的商務需求和（或）持續時間（等）。

* [如何建立和使用標記](httphttps://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11：使用自動化工具來監視網路資源設定並偵測變更

**指引**：使用 Azure 活動記錄來監視網路資源設定，並偵測與 Azure Databricks 實例相關的網路資源變更。 在 Azure 監視器中建立警示，以在重大網路資源的變更發生時觸發。

* [如何查看和取出 Azure 活動記錄事件](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [如何在 Azure 監視器中建立警示](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="logging-and-monitoring"></a>記錄和監視

*如需詳細資訊，請參閱[安全性控制：記錄和監視](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring)。*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1：使用已核准的時間同步處理來源

**指引**： Microsoft 會維護 Azure 資源的時間來源，不過，您可以選擇管理計算資源的時間同步處理設定。 因為 Azure Databricks 是 PaaS 服務，所以您無法直接存取 Azure Databricks 叢集中的 Vm，也無法設定時間同步處理設定。

**Azure 資訊安全中心監視**：不適用

**責任**： Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2：設定中央安全性記錄管理

**指導**方針：透過 Azure 監視器內嵌記錄，以匯總 Azure Databricks 所產生的安全性資料。 在 Azure 監視器中，您可以查詢 Log Analytics 工作區，其設定為接收您的 Databricks 和診斷記錄。 使用適用于長期/封存記錄儲存體或事件中樞的 Azure 儲存體帳戶，將資料匯出至其他系統。 或者，您可以啟用和內部資料，以 Azure Sentinel 或協力廠商安全性事件和事件管理（SIEM）。

注意： Azure Databricks 診斷記錄需要 Azure Databricks Premium 方案

* [如何設定診斷設定](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings#create-diagnostic-settings-in-azure-portal)

* [如何上架 Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [如何開始使用 Azure 監視器和協力廠商 SIEM 整合](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3：啟用 Azure 資源的審核記錄

**指引**：啟用 Azure 活動記錄診斷設定，並將記錄傳送到 Log Analytics 工作區、azure 事件中樞或 azure 儲存體帳戶進行封存。 使用 Azure 活動記錄資料，您可以判斷在 Azure 資源的控制平面層級執行的任何寫入作業（PUT、POST、DELETE）的「內容、物件和時間」。

針對審核記錄，Azure Databricks 提供 Azure Databricks 使用者所執行活動的完整端對端診斷記錄，讓您的企業能夠監視詳細的 Azure Databricks 使用模式。

注意： Azure Databricks 診斷記錄需要 Azure Databricks Premium 方案

* [如何啟用 Azure 活動記錄的診斷設定](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)）

* [如何啟用 Azure Databricks 的診斷設定](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4：從作業系統收集安全性記錄

**指導**方針： Azure Databricks 提供 Azure Databricks 使用者所執行活動的完整端對端診斷記錄，讓您的企業能夠監視詳細的 Azure Databricks 使用模式。

注意： Azure Databricks 診斷記錄需要 Azure Databricks Premium 方案。 無法使用 OS 安全性記錄。

* [如何啟用 Azure 活動記錄的診斷設定](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [如何啟用 Azure Databricks 的診斷設定](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="25-configure-security-log-storage-retention"></a>2.5：設定安全性記錄儲存體保留

**指引**：啟用 Azure Databricks 的診斷設定。 如果選擇將記錄儲存在 Log Analytics 工作區中，請根據貴組織的合規性法規來設定您的 Log Analytics 工作區保留期間。 針對長期/封存儲存體使用 Azure 儲存體帳戶。 安全性相關活動會在 Databricks audit 記錄中進行追蹤。

注意： Azure Databricks 診斷記錄需要 Azure Databricks Premium 方案

* [如何在 Azure Databricks 中啟用診斷設定](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs)

* [如何設定 Log Analytics 工作區的記錄檔保留參數](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="26-monitor-and-review-logs"></a>2.6：監視和審核記錄

**指引**：啟用 Azure Databricks 的診斷設定，並將記錄傳送到 Log Analytics 工作區。 使用 Log Analytics 工作區來分析和監視您的 Azure Databricks 記錄，以取得異常行為，並定期查看結果。

或者，您可以啟用和內部資料，以 Azure Sentinel 或協力廠商 SIEM。

注意： Azure Databricks 診斷記錄需要 Azure Databricks Premium 方案

* [如何在 Azure Databricks 中啟用診斷設定](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs)

* [如何查詢傳送至 Log Analytics 工作區 Azure Databricks 記錄](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs#analyze-diagnostic-logs)

* [如何上架 Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7：啟用異常活動的警示

**指導**方針：設定 Azure Databricks 實例的診斷設定，並將記錄傳送到 Log Analytics 工作區。 在您的 Log Analytics 工作區中，設定在一組預先定義的條件發生時，所要採取的警示。

或者，您可以啟用和內部資料，以 Azure Sentinel 或協力廠商 SIEM。

注意： Azure Databricks 診斷記錄需要 Azure Databricks Premium 方案

* [如何將 Azure Databricks 記錄傳送至 Log Analytics 工作區](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs#configure-diagnostic-log-delivery)）

* [如何在 Log Analytics 工作區中設定警示](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="28-centralize-anti-malware-logging"></a>2.8：集中反惡意程式碼記錄

**指導**方針：不適用。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="29-enable-dns-query-logging"></a>2.9：啟用 DNS 查詢記錄

**指導**方針：不適用;Azure Databricks 不會處理或產生使用者可存取的 DNS 相關記錄。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="210-enable-command-line-audit-logging"></a>2.10：啟用命令列審核記錄

**指導**方針：不適用;此指導方針適用于計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

## <a name="identity-and-access-control"></a>身分識別與存取控制

*如需詳細資訊，請參閱[安全性控制：身分識別和存取控制](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control)。*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1：維護系統管理帳戶的清查

**指導**方針：您可以使用 Azure Databricks SCIM api 來管理 Azure Databricks 工作區中的使用者，並將系統管理許可權授與指定的使用者。 您也可以透過 Azure Databricks UI 來管理它們。

* [如何使用 SCIM Api](https://docs.microsoft.com/azure/databricks/dev-tools/api/latest/scim/)

* [如何在 Azure Databricks 中新增和移除使用者](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/users)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="32-change-default-passwords-where-applicable"></a>3.2：變更預設密碼（若適用）

**指導**方針： Azure Databricks 使用 AZURE ACTIVE DIRECTORY （AD）來提供 Azure 入口網站以及 Azure Databricks 管理主控台的存取權。 Azure AD 沒有預設密碼的概念，但是您必須負責變更或不允許任何自訂或協力廠商應用程式的預設密碼。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="33-use-dedicated-administrative-accounts"></a>3.3：使用專用的系統管理帳戶

**指導**方針：您可以使用 Azure Databricks SCIM api，在 Azure Databricks 中新增具有系統管理員許可權的使用者。 您也可以透過 Azure Databricks UI 來管理它們。

* [如何使用 SCIM Api](https://docs.microsoft.com/azure/databricks/dev-tools/api/latest/scim/)

* [如何在 Azure Databricks 中新增和移除使用者](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/users)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4：搭配 Azure Active Directory 使用單一登入（SSO）

**指導**方針： Azure Databricks 會自動設定為使用 Azure Active Directory 單一登入來驗證使用者。 您組織外部的使用者必須完成邀請程式，並新增至您的 Active Directory 租使用者，他們才能透過單一登入登入 Azure Databricks。 您可以執行 SCIM，從工作區自動布建和取消布建使用者。

* [如何使用 SCIM Api](https://docs.microsoft.com/azure/databricks/dev-tools/api/latest/scim/)

* [瞭解 Azure Databricks 的單一登入](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/single-sign-on/)

* [如何邀請使用者加入您的 Azure AD 租使用者](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-external-users)

**Azure 資訊安全中心監視**：不適用

**責任**： Microsoft

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5：針對所有以 Azure Active Directory 為基礎的存取，請使用多重要素驗證。

**指導**方針：啟用 Azure AD MFA，並遵循 Azure 資訊安全中心身分識別和存取管理建議。

* [如何在 Azure 中啟用 MFA](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

* [如何監視 Azure 資訊安全中心內的身分識別和存取](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6：使用專用電腦（特殊許可權存取工作站）進行所有系統管理工作

**指導**方針：搭配使用 paw （特殊許可權存取工作站）與設定為登入和設定 Azure 資源的 MFA。

* [瞭解特殊許可權存取工作站](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [如何在 Azure 中啟用 MFA](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7：從系統管理客戶紀錄和警示可疑活動

**指導**方針：當環境中發生可疑或不安全的活動時，請使用 Azure Active Directory 安全性報告來產生記錄檔和警示。 使用 Azure 資訊安全中心來監視身分識別和存取活動。 此外，您可以利用 Azure Databricks 診斷記錄。

* [如何識別標示有風險活動 Azure AD 使用者](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk)

* [如何在 Azure 資訊安全中心中監視使用者的身分識別和存取活動](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8：僅從核准的位置管理 Azure 資源

**指導**方針：使用名為「位置」的條件式存取，只允許來自 IP 位址範圍或國家/地區特定邏輯群組的存取權。

* [如何在 Azure 中設定命名位置](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="39-use-azure-active-directory"></a>3.9：使用 Azure Active Directory

**指導**方針： Azure Databricks 會自動設定為使用 Azure Active Directory 單一登入來驗證使用者。 您組織外部的使用者必須完成邀請程式，並新增至您的 Active Directory 租使用者，他們才能透過單一登入登入 Azure Databricks。

* [瞭解 Azure Databricks 的單一登入](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/single-sign-on/)

* [如何邀請使用者加入您的 Azure AD 租使用者](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-external-users)

**Azure 資訊安全中心監視**：不適用

**責任**： Microsoft

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10：定期審查並協調使用者存取

**指導**方針： Azure AD 提供記錄檔，以協助探索過時的帳戶。 此外，您可以使用 Azure 身分識別存取審查來有效率地管理群組成員資格、企業應用程式的存取權，以及角色指派。 您可以定期檢查使用者存取權，以確保只有適當的使用者可以繼續進行存取。 您也可以執行 SCIM Api，並 Azure Databricks 診斷記錄來審查使用者存取權。 您也可以使用 SCIM Api 和 Azure Databricks 診斷記錄來審查使用者存取權。

此外，也會在 Azure Databricks 管理主控台內，定期檢查和管理使用者存取權。

* [Azure AD 報告](https://docs.microsoft.com/azure/active-directory/reports-monitoring/)

* [如何使用 Azure 身分識別存取評論](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

* [如何在 Azure Databricks 管理主控台中管理使用者存取權](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/users)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11：監視嘗試存取停用的帳戶

**指引**：您可以存取 Azure AD 登入活動、Audit 和風險事件記錄檔來源，讓您可以與任何 SIEM/監視工具整合。 此外，您可以使用 Azure Databricks 診斷記錄來審查使用者存取活動。

若要簡化此程式，您可以建立 Azure Active Directory 使用者帳戶的診斷設定，並將 audit 記錄和登入記錄傳送到 Log Analytics 工作區。 您可以在 Log Analytics 工作區中設定所需的警示。

* [如何使用 SCIM Api](https://docs.microsoft.com/azure/databricks/dev-tools/api/latest/scim/)

* [如何將 Azure 活動記錄整合到 Azure 監視器](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12：帳戶登入行為偏差的警示

**指導**方針：使用 Azure AD 風險和身分識別保護功能，為偵測到與使用者身分識別相關的可疑動作設定自動回應。 您也可以將資料內嵌到 Azure Sentinel，以進行進一步的調查。 此外，您可以使用 Azure Databricks 診斷記錄來審查使用者存取活動。

* [如何觀看 Azure AD 有風險的登入](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

* [如何設定和啟用身分識別保護風險原則](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

* [如何上架 Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13：提供 Microsoft 在支援案例期間存取相關的客戶資料

**指導**方針：當支援票證開啟時，客戶服務和支援工程師會要求同意存取相關的客戶資料。

**Azure 資訊安全中心監視**：目前無法使用

**責任**：目前無法使用

## <a name="data-protection"></a>資料保護

*如需詳細資訊，請參閱[安全性控制：資料保護](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection)。*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1：維護機密資訊的清查

**指引**：使用標記來協助追蹤處理敏感資訊的 Azure Databricks 實例。

* [如何建立和使用標記](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2：隔離儲存或處理敏感資訊的系統

**指引**：在開發、測試和生產環境中，執行不同的訂用帳戶和/或管理群組。 Azure Databricks 的預設部署是部署在自己的虛擬網路內的完全受控服務。 如果您需要網路自訂，您可以在自己的虛擬網路中部署 Azure Databricks。 最佳做法是為不同的商務團隊或部門建立個別的 Azure Databricks 工作區。

* [如何將 Azure Databricks 部署到您自己的虛擬網路](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)

* [如何建立額外的 Azure 訂用帳戶](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [如何建立管理群組](https://docs.microsoft.com/azure/governance/management-groups/create)

* [如何建立虛擬網路](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3：監視並封鎖未經授權的機密資訊傳輸。

**指導**方針：遵循 Databricks 的外泄保護架構，以減輕資料外泄的可能性。

* [Azure Databricks 的資料外泄保護](https://databricks.com/blog/2020/03/27/data-exfiltration-protection-with-azure-databricks.html)

Microsoft 會管理 Azure Databricks 的基礎結構，並已實行嚴格的控制，以避免遺失或公開客戶資料。

* [瞭解 Azure 中的客戶資料保護](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：目前無法使用

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4：加密傳輸中的所有機密資訊

**指導**方針： Microsoft 會在透過 Azure 入口網站或 Azure Databricks 主控台管理您的 Azure Databricks 實例時，預設協商 TLS 1.2。 Databricks Web 應用程式支援 TLS 1.3。

**Azure 資訊安全中心監視**：不適用

**責任**： Microsoft

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5：使用 active discovery 工具來識別敏感性資料

**指導**方針：目前無法使用;Azure Databricks 目前無法使用資料識別、分類和遺失防護功能。 標記 Azure Databricks 實例和相關的資源，可能會處理敏感資訊，並在必要時執行協力廠商解決方案，以符合規範目的。

Databricks 平臺為僅限計算，而且所有資料都儲存在其他 Azure 資料服務上。 針對由 Microsoft 管理的基礎平臺，Microsoft 會將所有客戶內容視為機密，並移至絕佳的長度，以防範客戶資料遺失和暴露。 為了確保 Azure 中的客戶資料保持安全，Microsoft 已實行並維護一套強大的資料保護控制和功能。

* [瞭解 Azure 中的客戶資料保護](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：目前無法使用

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6：使用 Azure RBAC 來控制資源的存取權

**指引**：在 Azure Databricks 中，您可以使用存取控制清單（acl）來設定許可權，以存取資料表、叢集、集區、作業和工作區物件，例如筆記本、實驗和資料夾。 所有管理使用者都可以管理存取控制清單，就如同已獲得委派權限來管理存取控制清單的使用者一樣。 您可以使用 Azure RBAC 來設定 Azure Databricks 工作區的許可權。

注意：資料表、叢集、集區、作業和工作空間存取控制僅適用于 Azure Databricks Premium 方案

* [如何管理 Azure Databricks 中的存取控制](https://docs.microsoft.com/azure/databricks/administration-guide/access-control/)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7：使用以主機為基礎的資料遺失防護來強制存取控制

**指導**方針：不適用;這項建議適用于計算資源。

Microsoft 會管理 Azure Databricks 的基礎結構，並已實行嚴格的控制，以避免遺失或公開客戶資料。

* [瞭解 Azure 中的客戶資料保護](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8：加密機密資訊待用

**指導**方針： Azure Databricks 工作區包含管理平面，裝載于 Azure Databricks 管理的虛擬網路中，以及部署在客戶管理的虛擬網路中的資料平面。 控制平面會將所有使用者的筆記本和相關聯的筆記本結果儲存在資料庫中。 根據預設，所有的筆記本和結果都會使用不同的加密金鑰進行待用加密。

Databricks 檔案系統（DBFS）是裝載于 Azure Databricks 工作區中，並可在 Azure Databricks 叢集上使用的分散式檔案系統。 DBFS 會實作為您 Azure Databricks 工作區的受控資源群組中的儲存體帳戶。 根據預設，儲存體帳戶會使用 Microsoft 管理的金鑰進行加密。 您的資料會儲存在您擁有的 Azure 資料服務中，而且您可以選擇將其加密。 不建議使用 DBFS 來儲存生產資料

注意：這些功能不適用於所有 Azure Databricks 訂用帳戶。 請洽詢您的 Microsoft 或 Databricks 帳戶代表以要求存取權。

* [如何為 Azure Databricks 的筆記本啟用客戶管理的金鑰](https://docs.microsoft.com/azure/databricks/security/customer-managed-key-notebook)

* [如何啟用 Azure Databricks 檔案系統的客戶管理金鑰](https://docs.microsoft.com/azure/databricks/security/customer-managed-keys-dbfs)

**Azure 資訊安全中心監視**：不適用

**責任**：共用

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9：對重要 Azure 資源的變更進行記錄和警示

**指導**方針：使用 Azure 監視器搭配 Azure 活動記錄，以針對重大 Azure Databricks 工作區進行變更時，建立警示。

* [如何建立 Azure 活動記錄事件的警示](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="vulnerability-management"></a>弱點管理

*如需詳細資訊，請參閱[安全性控制：弱點管理](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)。*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1：執行自動化弱點掃描工具

**指引**：執行協力廠商弱點管理解決方案。

如果您有弱點管理平臺訂用帳戶，您可以使用 Azure Databricks 初始化腳本，在您的 Azure Databricks 叢集節點上安裝弱點評定代理程式，並透過個別的入口網站管理節點。 請注意，每個協力廠商解決方案的運作方式不同。

* [如何手動安裝 Rapid7 代理程式](https://insightagent.help.rapid7.com/docs/install)

* [如何手動安裝 Qualys 代理程式](https://www.qualys.com/docs/qualys-cloud-agent-linux-install-guide.pdf)

* [Azure Databricks 初始化腳本](https://docs.microsoft.com/azure/databricks/clusters/init-scripts)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2：部署自動化的作業系統修補程式管理解決方案

**指引**： Microsoft 會維護 Azure Databricks 叢集節點基底映射，不過，您必須負責確保您的叢集節點保持修補。 若要將維護更新新增至現有執行中的叢集，您必須重新開機叢集。

* [瞭解 Azure Databricks 的執行時間維護更新](https://docs.microsoft.com/azure/databricks/release-notes/runtime/maintenance-updates)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3：部署自動化的協力廠商軟體修補程式管理解決方案

**指引**： Microsoft 會維護 Azure Databricks 叢集節點基底映射，不過您必須負責確保您安裝的任何協力廠商應用程式都能保持修補。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4：比較回溯弱點掃描

**指引**：執行協力廠商弱點管理解決方案，其能夠比較一段時間的弱點掃描。 如果您有弱點管理訂用帳戶，您可以使用該廠商的入口網站來查看和比較回溯弱點掃描。 請注意，每個協力廠商解決方案的運作方式不同。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5：使用風險評等程式來優先處理探索到的弱點。

**指導**方針：使用常見的風險評分計畫（例如，常見的弱點評分系統）或協力廠商掃描工具所提供的預設風險評等。

**Azure 資訊安全中心監視**： N/A

**責任**：客戶

## <a name="inventory-and-asset-management"></a>清查和資產管理

*如需詳細資訊，請參閱[安全性控制：清查和資產管理](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management)。*

### <a name="61-use-azure-asset-discovery"></a>6.1：使用 Azure 資產探索

**指引**：使用 Azure Resource Graph 來查詢/探索訂用帳戶內的所有資源（例如計算、儲存體、網路、埠及通訊協定等）。 請確定您的租使用者中有適當的（讀取）許可權，並列舉訂用帳戶中的所有 Azure 訂用帳戶及資源。

雖然可透過 Resource Graph 探索傳統的 Azure 資源，但強烈建議您建立並使用 Azure Resource Manager 的資源。

* [如何使用 Azure Resource Graph 建立查詢](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

* [如何查看您的 Azure 訂用帳戶](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

* [瞭解 Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="62-maintain-asset-metadata"></a>6.2：維護資產中繼資料

**指引**：將標籤套用至 Azure 資源，讓中繼資料以邏輯方式將其組織成分類法。

* [如何建立和使用標記](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="63-delete-unauthorized-azure-resources"></a>6.3：刪除未經授權的 Azure 資源

**指引**.. 適當地使用標記、管理群組和個別訂用帳戶來組織和追蹤 Azure 資源。 定期協調清查，並確保未經授權的資源會及時從訂用帳戶中刪除。

此外，使用下列內建原則定義，透過 Azure 原則對可在客戶訂用帳戶中建立的資源類型施加限制：
- 不允許的資源類型
- 允許的資源類型

* [如何建立額外的 Azure 訂用帳戶](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [如何建立管理群組](https://docs.microsoft.com/azure/governance/management-groups/create)

* [如何建立和使用標記](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="64-maintain-inventory-of-approved-azure-resources-and-software-titles"></a>6.4：維護已核准的 Azure 資源和軟體標題的清查。

**指導**方針：定義已核准的 Azure 資源，以及適用于計算資源的已核准軟體。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5：未核准的 Azure 資源的監視

**指引**：使用下列內建原則定義，透過 Azure 原則對可在客戶訂用帳戶中建立的資源類型施加限制：
- 不允許的資源類型
- 允許的資源類型

使用 Azure Resource Graph 來查詢/探索其訂用帳戶內的資源。 請確定已核准環境中的所有 Azure 資源。

* [如何設定和管理 Azure 原則](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [如何使用 Azure Graph 建立查詢](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6：監視計算資源內未經核准的軟體應用程式

**指導**方針：不適用;Azure Databricks 是 PaaS 服務，客戶無法直接存取 Azure Databricks 叢集中的 Vm。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7：移除未核准的 Azure 資源和軟體應用程式

**指引**：使用 Azure Resource Graph 來查詢/探索訂用帳戶內的所有資源（例如計算、儲存體、網路、埠和通訊協定等），包括 Azure Databricks 實例。 移除您探索到的任何未核准 Azure 資源。 針對 Azure Databricks 叢集節點，請執行協力廠商解決方案，以移除或警示未核准的軟體。

* [如何使用 Azure Graph 建立查詢](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="68-use-only-approved-applications"></a>6.8：僅使用已核准的應用程式

**指導**方針：不適用;Azure Databricks 是 PaaS 服務，您無法直接存取 Azure Databricks 叢集中的 Vm。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="69-use-only-approved-azure-services"></a>6.9：僅使用已核准的 Azure 服務

**指引**：使用 Azure 原則來對可使用下列內建原則定義在客戶訂用帳戶中建立的資源類型進行限制：
- 不允許的資源類型
- 允許的資源類型

* [如何設定和管理 Azure 原則](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [如何使用 Azure 原則拒絕特定的資源類型](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="610-implement-approved-application-list"></a>6.10：執行核准的應用程式清單

**指導**方針：不適用;Azure Databricks 是 PaaS 服務，您無法直接存取 Azure Databricks 叢集中的 Vm。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="611-divlimit-users-ability-to-interact-with-azure-resource-manager-via-scriptsdiv"></a>6.11： <div>限制使用者透過腳本與 Azure Resource Manager 互動的能力</div>

**指引**：使用 Azure 條件式存取，藉由設定「Microsoft Azure 管理」應用程式的「封鎖存取」，限制使用者與 Azure Resource Manager 互動的能力。

* [如何設定條件式存取以封鎖 Azure Resource Manager 的存取](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12：限制使用者在計算資源內執行腳本的能力

**指導**方針：不適用;這不適用於 Azure Databricks 因為叢集的使用者（非系統管理員）不需要存取個別節點來執行作業。 根據預設，叢集系統管理員具有所有叢集節點的根存取權。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13：實際或邏輯上隔離高風險應用程式

**指導**方針：不適用;基準測試適用于裝載 web 應用程式的 Azure 應用程式服務或計算資源。

**Azure 資訊安全中心監視**：不適用

**責任**：不適用

## <a name="secure-configuration"></a>安全設定

*如需詳細資訊，請參閱[安全性控制：安全](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration)設定。*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1：為所有 Azure 資源建立安全設定

**指引**：使用 Azure 原則為您的 Azure Databricks 實例定義和執行標準安全性設定。 使用 "Databricks" 命名空間中 Azure 原則別名來建立自訂原則，以審核或強制執行 Azure Databricks 實例的設定。 請注意，套用的任何原則都無法在 Databricks 受控資源群組上使用。

* [如何查看可用的 Azure 原則別名](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [如何設定和管理 Azure 原則](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="72-establish-secure-operating-system-configurations"></a>7.2：建立安全的作業系統設定

**指導**方針：不適用;Azure Databricks 是 PaaS 服務，您無法直接存取 Azure Databricks 叢集中的 Vm。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3：維護安全的 Azure 資源設定

**指引**：使用 Azure 原則為您的 Azure Databricks 實例定義和執行標準安全性設定。 使用 "Databricks" 命名空間中 Azure 原則別名來建立自訂原則，以審核或強制執行 Azure Databricks 實例的設定。 使用 Azure 原則 [拒絕] 和 [在不存在時部署]，在您的 Azure 資源上強制執行安全設定。

* [如何設定和管理 Azure 原則](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [瞭解 Azure 原則效果](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4：維護安全的作業系統設定

**指引**： Azure Databricks 由 Microsoft 所管理和維護的作業系統映射。 您必須負責執行 OS 層級狀態設定。

**Azure 資訊安全中心監視**：不適用

**責任**：共用

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5：安全地儲存 Azure 資源的設定

**指導**方針：如果使用自訂的 Azure 原則定義，請使用 Azure DevOps 或 Azure Repos 來安全地儲存和管理您的程式碼。

* [如何將程式碼儲存在 Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Azure Repos 檔](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="76-securely-store-custom-operating-system-images"></a>7.6：安全地儲存自訂的作業系統映射

**指導**方針：如果您的 Azure Databricks 叢集節點使用自訂映射，請將您的自訂基底映射推送至 Docker 登錄，例如 AZURE CONTAINER REGISTRY （ACR）。

* [如何使用 Databricks 容器服務自訂容器](https://docs.microsoft.com/azure/databricks/clusters/custom-containers)

* [瞭解 Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-intro)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="77-deploy-system-configuration-management-tools"></a>7.7：部署系統設定管理工具

**指引**：使用 Azure 原則為您的 Azure Databricks 實例定義和執行標準安全性設定。 使用 "Databricks" 命名空間中 Azure 原則別名來建立自訂原則，以審核或強制執行 Azure Databricks 實例的設定。

* [如何設定和管理 Azure 原則](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [如何查看可用的 Azure 原則別名](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [如何設定和管理 Azure 原則](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8：部署作業系統的系統建構管理工具

**指導**方針：不適用;Azure Databricks 是 PaaS 服務，您無法直接存取 Azure Databricks 叢集中的 Vm。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9：執行 Azure 服務的自動化設定監視

**指引**：使用 Azure 原則為您的 Azure Databricks 實例定義和執行標準安全性設定。 使用 "Databricks" 命名空間中 Azure 原則別名來建立自訂原則，以審核或強制執行 Azure Databricks 實例的設定。

* [如何查看可用的 Azure 原則別名](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [如何設定和管理 Azure 原則](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10：為作業系統執行自動化設定監視

**指引**：執行協力廠商解決方案，以監視 Azure Databricks 叢集節點作業系統的狀態。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="711-manage-azure-secrets-securely"></a>7.11：安全地管理 Azure 秘密

**指導**方針：使用 Azure Key Vault 搭配 Azure Databricks 秘密範圍，以安全地管理及使用秘密。

* [如何搭配 Azure Databricks 使用 Azure Key Vault](https://docs.microsoft.com/azure/azure-databricks/store-secrets-azure-key-vault)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="712-manage-identities-securely-and-automatically"></a>7.12：安全且自動地管理身分識別

**指導**方針：目前無法使用;受控識別目前無法供 Azure Databricks

* [支援適用於 Azure 資源的受控識別服務](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：目前無法使用

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13：消除非預期的認證暴露

**指導**方針：執行認證掃描器來識別程式碼中的認證。 認證掃描器也鼓勵將探索到的認證移至更安全的位置，例如 Azure Key Vault。

* [如何設定認證掃描器](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="malware-defense"></a>惡意程式碼防禦

*如需詳細資訊，請參閱[安全性控制：惡意程式碼防護](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense)。*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1：使用集中管理的反惡意程式碼軟體

**指導**方針：如果您有弱點管理平臺訂用帳戶，您可以使用 Azure Databricks 初始化腳本，在您的 Azure Databricks 叢集節點上安裝弱點評定代理程式，並透過個別的入口網站管理節點。 請注意，每個協力廠商解決方案的運作方式不同。

* [如何手動安裝 Rapid7 代理程式](https://insightagent.help.rapid7.com/docs/install)

* [如何手動安裝 Qualys 代理程式](https://www.qualys.com/docs/qualys-cloud-agent-linux-install-guide.pdf)

* [Azure Databricks 初始化腳本](https://docs.microsoft.com/azure/databricks/clusters/init-scripts)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2：預先掃描要上傳至非計算 Azure 資源的檔案

**指引**：在支援 Azure 服務的基礎主機（例如 Azure App Service）上啟用 Microsoft Antimalware，但不會在您的內容上執行。

預先掃描要上傳至 Azure Databricks 叢集節點或相關資源的任何檔案。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3：確認反惡意程式碼軟體和簽章已更新

**指導**方針：如果您有弱點管理平臺訂用帳戶，您可以使用 Azure Databricks 初始化腳本，在您的 Azure Databricks 叢集節點上安裝弱點評定代理程式，並透過個別的入口網站管理節點。 請注意，每個協力廠商解決方案的運作方式不同。

* [如何手動安裝 Rapid7 代理程式](https://insightagent.help.rapid7.com/docs/install)

* [如何手動安裝 Qualys 代理程式](https://www.qualys.com/docs/qualys-cloud-agent-linux-install-guide.pdf)

* [Azure Databricks 初始化腳本](https://docs.microsoft.com/azure/databricks/clusters/init-scripts)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="data-recovery"></a>資料復原

*如需詳細資訊，請參閱[安全性控制：資料](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery)復原。*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1：確保週期性自動備份

**指導**方針：針對您的 Azure Databricks 資料來源，請確定您已針對您的使用案例設定適當的資料冗余層級。 例如，如果您 Azure Databricks 資料存放區使用 Azure 儲存體帳戶，請選擇適當的 [冗余] 選項（[LRS]、[ZRS]、[GRS]、[RA-GRS]）。

* [Azure Databricks 的資料來源](https://docs.microsoft.com/azure/azure-databricks/databricks-connect-to-data-sources#data-sources-for-azure-databricks)

* [Azure Databricks 叢集的區域性災害復原](https://docs.microsoft.com/azure/azure-databricks/howto-regional-disaster-recovery)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2：執行完整的系統備份，並備份任何客戶管理的金鑰

**指引**：在 Azure Key Vault 內備份與您的 Azure Databricks 的任何相關的客戶管理金鑰。 您也可以使用 REST API 和 CLI 來建立 Databricks 設定的每日備份。 您也可以使用 REST API/CLI 來建立 Databricks 設定的每日備份。

* [如何在 Azure 中備份金鑰保存庫金鑰](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3：驗證所有備份，包括客戶管理的金鑰

**指導**方針：測試與您的 Azure Databricks 執行相關的已備份客戶管理金鑰的還原。

* [如何在 Azure 中還原金鑰保存庫金鑰](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4：確保備份和客戶管理金鑰的保護

**指導**方針：確定已在 Key Vault 中啟用虛刪除，以防止意外或惡意刪除的金鑰。

* [如何在 Key Vault 中啟用虛刪除](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Azure 資訊安全中心監視**：是

**責任**：客戶

## <a name="incident-response"></a>事件回應

*如需詳細資訊，請參閱[安全性控制：事件回應](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response)。*

### <a name="101-create-an-incident-response-guide"></a>10.1：建立事件回應指南

**指引**：為您的組織建立事件回應指南。 請確定有寫入的事件回應計畫，可定義人員的所有角色，以及從偵測到事件處理/管理的階段，以進行後續事件審查。

* [如何設定 Azure 資訊安全中心內的工作流程自動化](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)

* [建立您自己的安全性事件回應程式的指引](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Microsoft 安全性回應中心的事件剖析](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [您也可以利用 NIST 的「電腦安全性性」事件處理指南，協助建立自己的事件回應計畫](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2：建立事件評分和優先順序程式

**指導**方針：資訊安全中心指派每個警示的嚴重性，以協助您排定應先調查哪些警示。 嚴重性是根據資訊安全中心在尋找中的信心，或用於發出警示的分析，以及導致警示的活動背後有惡意意圖的信賴等級。

此外，請清楚地標示訂閱（例如， 生產、非生產）並建立命名系統，以清楚識別和分類 Azure 資源。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="103-test-security-response-procedures"></a>10.3：測試安全性回應程式

**指導**方針：執行練習以定期測試系統的事件回應功能。 識別弱式點和間距，並視需要修訂計畫。

* [請參閱 NIST 的發行集：適用于 IT 計畫和功能的測試、訓練和練習計劃指南](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4：提供安全性事件連絡人詳細資料，並設定安全性事件的警示通知

**指導**方針：如果 Microsoft 安全性回應中心（MSRC）發現非法或未經授權的合作物件已存取客戶資料，microsoft 將會使用安全性事件連絡人資訊來與您聯繫。 檢查事實後的事件，以確保解決問題。

* [如何設定 Azure 資訊安全中心安全性連絡人](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5：將安全性警示納入事件回應系統中

**指引**：使用「連續匯出」功能來匯出您的 Azure 資訊安全中心警示和建議。 「連續匯出」可讓您以手動或持續的方式來匯出警示和建議。 您可以使用 Azure 資訊安全中心資料連線器來串流「警示」 Sentinel。

* [如何設定連續匯出](https://docs.microsoft.com/azure/security-center/continuous-export)

* [如何將警示串流至 Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="106-automate-the-response-to-security-alerts"></a>10.6：自動回應安全性警示

**指引**：使用 Azure 資訊安全中心中的工作流程自動化功能，透過「Logic Apps」安全性警示和建議，自動觸發回應。

* [如何設定工作流程自動化和 Logic Apps](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="penetration-tests-and-red-team-exercises"></a>滲透測試和 Red Team 練習

*如需詳細資訊，請參閱[安全性控制：滲透測試和 red team 練習](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises)。*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11.1：進行 Azure 資源的定期滲透測試，並確保在60天內補救所有重大安全性結果。

**指導**方針： *[遵循 microsoft Engagement 規則，確保您的滲透測試不會違反 microsoft 原則](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

* [您可以在這裡找到有關 Microsoft 管理的雲端基礎結構、服務和應用程式的 Microsoft 策略及執行的 Red 小組和即時網站滲透測試的詳細資訊](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure 資訊安全中心監視**：不適用

**責任**：共用

## <a name="next-steps"></a>後續步驟

- 請參閱[Azure 安全性基準測試](https://docs.microsoft.com/azure/security/benchmarks/overview)
- 深入瞭解[Azure 安全性基準](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)
