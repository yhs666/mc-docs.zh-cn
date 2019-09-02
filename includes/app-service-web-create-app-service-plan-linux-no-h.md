使用 [az appservice plan create](/cli/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) 命令在资源组中创建应用服务计划。

<!-- [!INCLUDE [app-service-plan](app-service-plan-linux.md)] -->

以下示例在**基本**定价层 (`--sku B1`) 和 Linux 容器 (`--is-linux`) 中创建名为 `myAppServicePlan` 的应用服务计划。

```azurecli
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1 --is-linux
```

创建应用服务计划后，Azure CLI 将显示类似于以下示例的信息：

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "China East",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "linux",
  "location": "China East",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```