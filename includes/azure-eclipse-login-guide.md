> [!NOTE]
> 若要使用 Azure 工具包登录 Azure 中国，首先需要准备服务主体的身份验证文件。
>
> 通过 Azure CLI 2.0 可轻松创建服务主体并授予其对给定订阅的访问特权。
>
> 1. 在登录前使用 `az cloud set -n AzureChinaCloud` 命令切换到 Azure 中国。
> 1. 通过运行命令 `az login` 以用户身份登录。
> 1. 通过运行 `az account set --subscription <subscription name>` 选择想要服务主体有权对其进行访问的订阅。 可通过 `az account list --out jsonc` 查看订阅。
> 1. 运行以下命令创建服务主体身份验证文件。
> 
>     Bash：
>     ```
>     curl -L https://raw.githubusercontent.com/Azure/azure-sdk-for-java/master/tools/authgen.py | python > my.azureauth
>     ```
>     PowerShell：
>     ```
>     Invoke-RestMethod -Method GET -Uri  https://raw.githubusercontent.com/Azure/azure-sdk-for-java/master/tools/authgen.py | python | Out-File my.azureauth
>     ```
>
> 1. 若要通过 Azure 工具包登录 Azure 中国，请使用 `Automated` 而不是 `Interactive`。
>
>     ![azure 登录](./media/azure-eclipse-login-guide/azure-sign-in.png)