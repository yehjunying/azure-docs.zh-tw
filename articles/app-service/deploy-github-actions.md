---
title: 使用 GitHub 動作來設定 CI/CD
description: 瞭解如何使用 GitHub 動作，從 CI/CD 管線將您的程式碼部署至 Azure App Service。 自訂群組建工作並執行複雜的部署。
ms.devlang: na
ms.topic: article
ms.date: 10/25/2019
ms.author: jafreebe
ms.reviewer: ushan
ms.openlocfilehash: 57ca5b0880d4b027e33bc0d01fc6225eb886029b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "82084986"
---
# <a name="deploy-to-app-service-using-github-actions"></a>使用 GitHub 動作部署到 App Service

[GitHub 動作](https://help.github.com/en/articles/about-github-actions)可讓您彈性地建立自動化軟體發展生命週期工作流程。 透過適用于 GitHub 的 Azure App Service 動作，您可以使用 GitHub 動作，將工作流程自動化，以部署至[Azure App Service](overview.md) 。

> [!IMPORTANT]
> GitHub 動作目前為搶鮮版（Beta）。 您必須先[註冊，才能](https://github.com/features/actions)使用您的 GitHub 帳戶加入預覽版。
> 

工作流程是由存放庫中`/.github/workflows/`路徑內的 YAML （. yml）檔案所定義。 此定義包含組成工作流程的各種步驟和參數。

若為 Azure App Service 工作流程，檔案有三個區段：

|區段  |工作  |
|---------|---------|
|**驗證** | 1. 定義服務主體 <br /> 2. 建立 GitHub 秘密 |
|**建置** | 1. 設定環境 <br /> 2. 建立 web 應用程式 |
|**部署** | 1. 部署 web 應用程式 |

## <a name="create-a-service-principal"></a>建立服務主體

您可以使用[Azure CLI](https://docs.microsoft.com/cli/azure/)中的[az ad sp create-rbac](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac)命令來建立[服務主體](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object)。 您可以使用 Azure 入口網站中的[Azure Cloud Shell](https://shell.azure.com/)或選取 [**試試看**] 按鈕來執行此命令。

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor --scopes /subscriptions/<subscription-id>/resourceGroups/<group-name>/providers/Microsoft.Web/sites/<app-name> --sdk-auth
```

在此範例中，將資源中的預留位置取代為您的訂用帳戶識別碼、資源組名和應用程式名稱。 輸出是可提供 App Service 應用程式存取權的角色指派認證。 複製這個 JSON 物件，您可以使用它從 GitHub 進行驗證。

> [!NOTE]
> 如果您決定使用發行設定檔進行驗證，則不需要建立服務主體。

> [!IMPORTANT]
> 授與最小存取權一律是最佳作法。 這就是上一個範例中的範圍限制為特定 App Service 應用程式，而不限於整個資源群組的原因。

## <a name="configure-the-github-secret"></a>設定 GitHub 秘密

您也可以使用應用層級的認證，也就是發行設定檔以進行部署。 請遵循下列步驟來設定密碼：

1. 使用 [**取得發行設定檔**] 選項，從入口網站下載 App Service 應用程式的發行設定檔。

2. 在[GitHub](https://github.com/)中，流覽您的存放庫、選取 [**設定] > 秘密 > 新增新密碼**

    ![密碼](media/app-service-github-actions/secrets.png)

3. 將下載的發行設定檔的內容貼入密碼的 [值] 欄位中。

4. 現在，在您分支中的工作流程`.github/workflows/workflow.yml`檔案中：取代 [部署`publish-profile` Azure Web 應用程式] 動作的輸入密碼。
    
    ```yaml
        - uses: azure/webapps-deploy@v2
          with:
            creds: ${{ secrets.azureWebAppPublishProfile }}
    ```

5. 在定義之後，您會看到如下所示的密碼。

    ![密碼](media/app-service-github-actions/app-service-secrets.png)

## <a name="set-up-the-environment"></a>設定 Azure 環境

設定環境可以使用其中一個安裝動作來完成。

|**語言**  |**設定動作**  |
|---------|---------|
|**.NET**     | `actions/setup-dotnet` |
|**Java**     | `actions/setup-java` |
|**JavaScript** | `actions/setup-node` |
|**Python**     | `actions/setup-python` |

下列範例顯示的工作流程部分，會針對各種支援的語言設定環境：

**JavaScript**

```yaml
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
```
**Python**

```yaml
    - name: Setup Python 3.6
      uses: actions/setup-python@v1
      with:
        python-version: 3.6
```

**.NET**

```yaml
    - name: Setup Dotnet 2.2.300
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: '2.2.300'
```

**Java**

```yaml
    - name: Setup Java 1.8.x
      uses: actions/setup-java@v1
      with:
        # If your pom.xml <maven.compiler.source> version is not in 1.8.x
        # Please change the Java version to match the version in pom.xml <maven.compiler.source>
        java-version: '1.8.x'
```

## <a name="build-the-web-app"></a>建立 web 應用程式

這取決於 Azure App Service 支援的語言和語言，本節應該是每種語言的標準組建步驟。

下列範例會以各種支援的語言，顯示建立 web 應用程式的工作流程部分。

**JavaScript**

```yaml
    - name: 'Run npm'
      shell: bash
      run: |
        # If your web app project is not located in your repository's root
        # Please change your directory for npm in pushd
        pushd .
        npm install
        npm run build --if-present
        npm run test --if-present
        popd
```

**Python**

```yaml
    - name: 'Run pip'
      shell: bash
      run: |
        # If your web app project is not located in your repository's root
        # Please change your directory for pip in pushd
        pushd .
        python -m pip install --upgrade pip
        pip install -r requirements.txt --target=".python_packages/lib/python3.6/site-packages"
        popd
```

**.NET**

```yaml
    - name: 'Run dotnet build'
      shell: bash
      run: |
        # If your web app project is not located in your repository's root
        # Please consider using pushd to change your path
        pushd .
        dotnet build --configuration Release --output ./output
        popd
```

**Java**

```yaml
    - uses: actions/checkout@v1
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    - name: Build with Maven
      run: mvn -B package --file pom.xml
```
## <a name="deploy-to-app-service"></a>部署到 App Service

若要將程式碼部署到 App Service 應用程式， `azure/webapps-deploy@v2`請使用動作。 此動作有四個參數：

| **參數**  | **說明**  |
|---------|---------|
| **應用程式名稱** | 具備App Service 應用程式的名稱 | 
| **發行-設定檔** | 選擇性發行具有 Web Deploy 秘密的設定檔檔案內容 |
| **套件** | 選擇性封裝或資料夾的路徑。 * .zip、* war、* .jar 或要部署的資料夾 |
| **位置名稱** | 選擇性輸入生產位置以外的現有插槽 |

### <a name="deploy-using-publish-profile"></a>使用發行設定檔進行部署

以下是使用發行設定檔建立 node.js 應用程式並將其部署至 Azure 的範例工作流程。

```yaml
# File: .github/workflows/workflow.yml

on: push

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout GitHub Action' 
      uses: actions/checkout@master
    
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
    - name: 'npm install, build, and test'
      run: |
        npm install
        npm run build --if-present
        npm run test --if-present
       
    - name: 'Run Azure webapp deploy action using publish profile credentials'
          uses: azure/webapps-deploy@v2
          with: 
            app-name: node-rn
            publish-profile: ${{ secrets.azureWebAppPublishProfile }}
```

### <a name="deploy-using-azure-service-principal"></a>使用 Azure 服務主體進行部署

以下是使用 Azure 服務主體來建立 node.js 應用程式並將其部署至 Azure 的範例工作流程。

```yaml
on: [push]

name: Node.js

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout GitHub Action' 
      uses: actions/checkout@master
   
    - uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
        
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
    
    - name: 'npm install, build, and test'
      run: |
        npm install
        npm run build --if-present
        npm run test --if-present
               
    # deploy web app using Azure credentials
    - uses: azure/webapps-deploy@v2
      with:
        app-name: 'node-rn'

    # Azure logout 
    - name: logout
      run: |
        az logout
```

## <a name="next-steps"></a>後續步驟

您可以在 GitHub 上找到分組到不同存放庫的一組動作，其中每一個都包含檔和範例，以協助您使用 GitHub 來進行 CI/CD，並將您的應用程式部署至 Azure。

- [要部署至 Azure 的動作工作流程](https://github.com/Azure/actions-workflow-samples)

- [Azure 登入](https://github.com/Azure/login)

- [Azure WebApp](https://github.com/Azure/webapps-deploy)

- [適用于容器的 Azure WebApp](https://github.com/Azure/webapps-container-deploy)

- [Docker 登入/登出](https://github.com/Azure/docker-login)

- [觸發工作流程的事件](https://help.github.com/en/articles/events-that-trigger-workflows)

- [K8s 部署](https://github.com/Azure/k8s-deploy)

- [入門工作流程](https://github.com/actions/starter-workflows)
