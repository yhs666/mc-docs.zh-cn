注销进程服务器的步骤各不相同，具体取决于其与配置服务器的连接状态。

### 注销处于“已连接”状态的进程服务器
<a id="unregister-a-process-server-that-is-in-a-connected-state" class="xliff"></a>

1. 以管理员身份远程进入进程服务器。
2. 启动“控制面板”，然后打开“程序”>“卸载程序”
3. 按名称 **Microsoft Azure Site Recovery 配置/进程服务器**
4. 步骤 3 完成后，可以卸载 **Microsoft Azure Site Recovery 配置/进程服务器依赖项**

### 注销处于“已断开连接 ”状态的进程服务器
<a id="unregister-a-process-server-that-is-in-a-disconnected-state" class="xliff"></a>

> [!WARNING]
> 如果无法恢复安装了进程服务器的虚拟机，则应使用以下步骤。

1. 以管理员身份登录配置服务器。
2. 打开管理命令提示符，然后浏览到目录 `%ProgramData%\ASR\home\svsystems\bin`。
3. 现在，运行命令。

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```

4. 这将从系统中清除进程服务器的详细信息。