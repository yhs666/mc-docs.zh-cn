使用 [az group create](https://docs.azure.cn/zh-cn/cli/webapp?view=azure-cli-latest#az_webapp_create) 命令创建资源组。

[!INCLUDE [resource group intro text](resource-group.md)]

以下示例在“chinanorth”位置创建名为“myResourceGroup”的资源组。 若要查看应用服务支持的所有位置，请运行 `az appservice list-locations` 命令。

```azurecli
az group create --name myResourceGroup --location chinanorth
```

通常在附近的区域中创建资源组和资源。 

此命令完成后，JSON 输出会显示资源组属性。