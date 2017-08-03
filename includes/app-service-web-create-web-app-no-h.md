使用 [az webapp create](https://docs.microsoft.com/cli/azure/webapp#create) 命令在 `myAppServicePlan` 应用服务计划中创建一个 [Web 应用](../articles/app-service-web/app-service-web-overview.md)。 

该 Web 应用为代码提供托管空间，并提供一个用于查看已部署应用的 URL。

在以下命令中，将 *\<app_name>* 替换为唯一名称（有效字符是 `a-z`、`0-9` 和 `-`）。 如果 `<app_name>` 不是唯一名称，将收到错误消息“具有给定名称 <app_name> 的网站已存在”。 Web 应用的默认 URL 为 `https://<app_name>.chinacloudsites.cn`。 

```azurecli
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

创建 Web 应用后，Azure CLI 将显示类似于以下示例的信息：

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.chinacloudsites.cn",
  "enabled": true,
  "enabledHostNames": [
    "<app_name>.chinacloudsites.cn",
    "<app_name>.scm.chinacloudsites.cn"
  ],
  "gatewaySiteName": null,
  "hostNameSslStates": [
    {
      "hostType": "Standard",
      "name": "<app_name>.chinacloudsites.cn",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "virtualIp": null
    }
    < JSON data removed for brevity. >
}
```

浏览到该站点查看新建的 Web 应用。

```bash
http://<app_name>.chinacloudsites.cn
```