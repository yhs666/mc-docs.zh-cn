> [!NOTE]
> 若要使用 Azure 工具包登录 Azure China，首先需要准备服务主体身份验证文件。
>
> 可轻松创建服务主体，并通过 Azure CLI 2.0 授予其对给定订阅的访问特权。
>
> 1. 在登录前，使用 `az cloud set -n AzureChinaCloud` 命令切换到 Azure China。
> 1. 通过运行命令 `az login`，以用户身份登录。
> 1. 通过运行 `az account set --subscription <subscription name>`，选择你希望服务主体可访问的订阅。 可通过 `az account list --out jsonc` 查看订阅。
> 1. 运行以下命令，创建服务主体身份验证文件。
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
> 1. 若要使用 Azure 工具包登录到 Azure China，请使用 `Automated`，而不是 `Interactive`。
>
>     ![azure-sign-in](./media/azure-intellij-login-guide/azure-sign-in.png)