## <a name="create-an-azure-storage-account"></a>创建 Azure 存储帐户

Functions 使用通用帐户在 Azure 存储中保留状态以及有关函数的其他信息。 使用 [az storage account create](/cli/storage/account#create) 命令在创建的资源组中创建通用存储帐户。

在以下命令中，请将 `<storage_name>` 占位符替换成全局唯一存储帐户名。 存储帐户名称必须为 3 到 24 个字符，并且只能包含数字和小写字母。

```azurecli
az storage account create --name <storage_name> --location chinanorth --resource-group myResourceGroup --sku Standard_LRS
```

创建存储帐户后，Azure CLI 会显示类似于以下示例的信息：

```json
{
  "creationTime": "2017-04-15T17:14:39.320307+00:00",
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myresourcegroup/...",
  "kind": "Storage",
  "location": "chinanorth",
  "name": "myfunctionappstorage",
  "primaryEndpoints": {
    "blob": "https://myfunctionappstorage.blob.core.chinacloudapi.cn/",
    "file": "https://myfunctionappstorage.file.core.chinacloudapi.cn/",
    "queue": "https://myfunctionappstorage.queue.core.chinacloudapi.cn/",
    "table": "https://myfunctionappstorage.table.core.chinacloudapi.cn/"
  },
     ....
    // Remaining output has been truncated for readability.
}
```

