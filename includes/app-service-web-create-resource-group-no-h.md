使用 [az group create](https://docs.microsoft.com/cli/azure/group#create) 命令创建资源组。

[!INCLUDE [resource group intro text](resource-group.md)]

以下示例在“chinanorth”位置创建名为“myResourceGroup”的资源组。

```azurecli
az group create --name myResourceGroup --location chinanorth
```

通常在附近的区域中创建资源组和资源。 若要查看 Azure Web 应用支持的所有位置，请运行 `az appservice list-locations` 命令。
