
[!INCLUDE [resource group intro text](resource-group.md)]

使用 [`az group create`](/cli/group?view=azure-cli-latest#az_group_create) 命令创建资源组。 以下示例在“中国北部”位置创建名为“myResourceGroup”的资源组。 要查看“免费”层中应用服务支持的所有位置，请运行 [`az appservice list-locations --sku F1`](/cli/appservice?view=azure-cli-latest#az_appservice_list_locations) 命令。

```azurecli
az group create --name myResourceGroup --location "China North"
```

通常在附近的区域中创建资源组和资源。 

此命令完成后，JSON 输出会显示资源组属性。