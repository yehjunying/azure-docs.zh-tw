---
title: 啟用和管理 blob 版本設定（預覽）
titleSuffix: Azure Storage
description: 瞭解如何在 Azure 入口網站中啟用 blob 版本設定，或使用 Azure Resource Manager 範本。
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 05/05/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 0e24bcb54fd26d4a3d983681b3348ef736b277cf
ms.sourcegitcommit: d815163a1359f0df6ebfbfe985566d4951e38135
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2020
ms.locfileid: "82884341"
---
# <a name="enable-and-manage-blob-versioning"></a>啟用和管理 blob 版本設定

您可以使用 Azure 入口網站或 Azure Resource Manager 範本，隨時為儲存體帳戶啟用或停用 blob 版本設定（預覽）。

## <a name="enable-blob-versioning"></a>啟用 blob 版本設定

# <a name="azure-portal"></a>[Azure 入口網站](#tab/portal)

若要在 Azure 入口網站中啟用 blob 版本設定：

1. 在入口網站中流覽至您的儲存體帳戶。
1. 在 [ **Blob 服務**] 下，選擇 [**資料保護**]。
1. 在 [**版本控制**] 區段中，選取 [**已啟用**]。

:::image type="content" source="media/versioning-enable/portal-enable-versioning.png" alt-text="顯示如何在 Azure 入口網站中啟用 blob 版本設定的螢幕擷取畫面":::

# <a name="template"></a>[範本](#tab/template)

若要使用範本啟用 blob 版本設定，請建立一個範本，並將**IsVersioningEnabled**屬性設定為**true**。 下列步驟說明如何在 Azure 入口網站中建立範本。

1. 在 [Azure 入口網站中，選擇 [**建立資源**]。
1. 在 [搜尋 Marketplace]**** 中，輸入**範本部署**，然後按 **ENTER**。
1. 選擇 [**範本部署**]，選擇 [**建立**]，然後**在編輯器中選擇 [建立您自己的範本**]。
1. 在 [範本編輯器] 中，貼上下列 JSON。 使用您的儲存體帳戶名稱取代 `<accountName>` 預留位置。
1. 儲存範本。
1. 指定帳戶的資源群組，然後選擇 [**購買**] 按鈕以部署範本並啟用 blob 版本設定。

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Storage/storageAccounts/blobServices",
                "apiVersion": "2019-06-01",
                "name": "<accountName>/default",
                "properties": {
                    "IsVersioningEnabled": true
                }
            }
        ]
    }
    ```

如需在 Azure 入口網站中使用範本部署資源的詳細資訊，請參閱[使用 Azure 入口網站部署資源](../../azure-resource-manager/templates/deploy-portal.md)。

---

## <a name="modify-a-blob-to-trigger-a-new-version"></a>修改 blob 以觸發新版本

下列程式碼範例示範如何使用適用于 .NET 第12版的 Azure 儲存體用戶端程式庫，觸發新版本的建立。 執行此範例之前，請確定您已針對儲存體帳戶啟用版本設定。

此範例會建立區塊 blob，然後更新 blob 的中繼資料。 更新 blob 的中繼資料會觸發新版本的建立。 此範例會抓取初始版本和目前的版本，並顯示只有目前的版本包含中繼資料。

```csharp
public static async Task UpdateVersionedBlobMetadata(string containerName, string blobName)
{
    // Create a new service client from the connection string.
    BlobServiceClient blobServiceClient = new BlobServiceClient(ConnectionString);

    // Create a new container client.
    BlobContainerClient containerClient = blobServiceClient.GetBlobContainerClient(containerName);

    try
    {
        // Create the container.
        await containerClient.CreateIfNotExistsAsync();

        // Upload a block blob.
        BlockBlobClient blockBlobClient = containerClient.GetBlockBlobClient(blobName);

        string blobContents = string.Format("Block blob created at {0}.", DateTime.Now);
        byte[] byteArray = Encoding.ASCII.GetBytes(blobContents);

        string initalVersionId;
        using (MemoryStream stream = new MemoryStream(byteArray))
        {
            Response<BlobContentInfo> uploadResponse = await blockBlobClient.UploadAsync(stream, null, default);

            // Get the version ID for the current version.
            initalVersionId = uploadResponse.Value.VersionId;
        }

        // Update the blob's metadata to trigger the creation of a new version.
        Dictionary<string, string> metadata = new Dictionary<string, string>
        {
            { "key", "value" },
            { "key1", "value1" }
        };

        Response<BlobInfo> metadataResponse = await blockBlobClient.SetMetadataAsync(metadata);

        // Get the version ID for the new current version.
        string newVersionId = metadataResponse.Value.VersionId;

        // Request metadata on the previous version.
        BlockBlobClient initalVersionBlob = blockBlobClient.WithVersion(initalVersionId);
        Response<BlobProperties> propertiesResponse = await initalVersionBlob.GetPropertiesAsync();
        PrintMetadata(propertiesResponse);

        // Request metadata on the current version.
        BlockBlobClient newVersionBlob = blockBlobClient.WithVersion(newVersionId);
        Response<BlobProperties> newPropertiesResponse = await newVersionBlob.GetPropertiesAsync();
        PrintMetadata(newPropertiesResponse);
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
    finally
    {
        await containerClient.DeleteAsync();
    }
}

static void PrintMetadata(Response<BlobProperties> propertiesResponse)
{
    if (propertiesResponse.Value.Metadata.Count > 0)
    {
        Console.WriteLine("Metadata values for version {0}:", propertiesResponse.Value.VersionId);
        foreach (var item in propertiesResponse.Value.Metadata)
        {
            Console.WriteLine("Key:{0}  Value:{1}", item.Key, item.Value);
        }
    }
    else
    {
        Console.WriteLine("Version {0} has no metadata.", propertiesResponse.Value.VersionId);
    }
}
```

## <a name="next-steps"></a>後續步驟

- [Blob 版本設定（預覽）](versioning-overview.md)
- [Azure 儲存體 Blob 的虛刪除](soft-delete-overview.md)
