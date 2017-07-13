| **硬件** | |
| --- |---|
| CPU 内核数| 8 |
| RAM| 12 GB|
| 磁盘数目 | 3 <br><br> - OS 磁盘<br> - 进程服务器缓存磁盘<br> - 保留驱动器（用于故障回复）|
| 磁盘可用空间（进程服务器缓存） | 600 GB
| 磁盘可用空间（保留磁盘） | 600 GB|
| **软件** | |
| 操作系统版本 | Windows Server 2012 R2 |
| 操作系统区域设置 | 英语 (en-us)|
| VMware vSphere PowerCLI 版本 | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0")|
| Windows Server 角色 | 请勿启用以下角色： <br> - Active Directory 域服务 <br>- Internet Information Services <br> - Hyper-V |
| **网络** | |
| 网络接口卡类型 | VMXNET3 |
| IP 地址类型 | 静态 |
| Internet 访问 | 服务器应能够直接或通过代理服务器访问以下 URL： <br> - \*.accesscontrol.chinacloudapi.cn<br> - \*.backup.windowsazure.cn <br>- \*.store.core.chinacloudapi.cn<br> - \*.blob.core.chinacloudapi.cn<br> - \*.hypervrecoverymanager.windowsazure.cn <br>  |
| 端口 | 443（控制通道协调）<br>9443（数据传输）|