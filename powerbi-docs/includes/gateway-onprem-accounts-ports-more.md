---
ms.openlocfilehash: e24218e2a465619fdfbfc279d3cc45370202dd6e
ms.sourcegitcommit: aef57ff94a5d452d6b54a90598bd6a0dd1299a46
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66814870"
---
## <a name="sign-in-account"></a>登录帐户

用户使用工作或学校帐户登录。 此帐户是你的组织帐户  。 如果你注册了 Office 365 产品/服务，但没有提供实际的工作电子邮件，则可能类似于 nancy@contoso.onmicrosoft.com。 你的帐户存储于 Azure Active Directory (AAD) 中的租户内。 在大多数情况下，你的 AAD 帐户的 UPN 将与电子邮件地址匹配。

## <a name="windows-service-account"></a>Windows 服务帐户

本地数据网关配置为使用 NT SERVICE\PBIEgwService 作为 Windows 服务登录凭据  。 默认情况下，它在安装网关所在的计算机上下文中具有作为服务登录的权限。 该帐户不是用于连接到本地数据源的同一帐户。 该帐户也不是登录到云服务的工作或学校帐户。

> [!NOTE]
> 如果选择了个人模式，则需单独配置 Windows 服务帐户。

如果你遇到有关代理服务器的身份验证问题，尝试将 Windows 服务帐户更改为域用户或托管服务帐户。 有关详细信息，请参阅[代理配置](../service-gateway-proxy.md#changing-the-gateway-service-account-to-a-domain-user)。

## <a name="ports"></a>端口

网关将创建与 Azure 服务总线的出站连接。 它可在以下出站端口上进行通信：TCP 443（默认）、5671、5672、9350 至 9354。  网关不需要入站端口。

建议将数据区域的 IP 地址添加到防火墙中的允许列表。 可以下载每周更新的 [Microsoft Azure 数据中心 IP 列表](https://www.microsoft.com/download/details.aspx?id=41653)。 或者，可以通过在本地数据网关应用程序上执行[网络端口测试](../service-gateway-onprem-tshoot.md#network-ports-test)来获取所需端口的列表。 网关使用 IP 地址以及完全限定的域名 (FQDN) 与 Azure 服务总线进行通信。 如果使用 HTTPS 强制网关进行通信，则网关将仅使用 FQDN，如果使用 IP 地址则不会进行通信。


> [!NOTE]
> Azure 数据中心 IP 列表中的 IP 地址以 CIDR 表示法列出。 例如，10.0.0.0/24 并不意味着 10.0.0.0 至 10.0.0.24。 了解有关 [CIDR 表示法](http://whatismyipaddress.com/cidr)的详细信息。

下面是网关使用的完全限定的域名的列表。

| 域名 | 出站端口 | 说明 |  |
|-----------------------------|----------------|--------------------------------------------------------------------------------------------------------------------|---|
| *.download.microsoft.com | 80 | 用于下载安装程序。 数据网关应用也使用它来检查版本和网关区域。 |  |
| *.powerbi.com | 443 | 用于标识相关的 Power BI 群集。 |  |
| *.analysis.windows.net | 443 | 用于标识相关的 Power BI 群集。 |  |
| *.login.windows.net | 443 | 用于通过 Azure Active Directory/OAuth2 对数据网关应用进行身份验证。 |  |
| *.servicebus.windows.net | 5671-5672 | 用于高级消息队列协议 (AMQP)。 |  |
| *.servicebus.windows.net | 443, 9350-9354 | 由基于 TCP 的服务总线中继上的侦听器（需要使用 443 获取访问控制令牌）使用。 |  |
| *.frontend.clouddatahub.net | 443 | 已弃用 - 不再需要。 将来会从文档中删除。 |  |
| *.core.windows.net | 443 | 由 Power BI 中的数据流用于将数据写入 Azure Data Lake。 |  |
| login.microsoftonline.com | 443 | 用于通过 Azure Active Directory/OAuth2 对数据网关应用进行身份验证。 |  |
| *.msftncsi.com | 443 | 用于测试 Internet 连接以及 Power BI 服务能否访问网关。 |  |
| *.microsoftonline-p.com | 443 | 用于通过 Azure Active Directory/OAuth2 对数据网关应用进行身份验证。 |  |
| | |

> [!NOTE]
> 安装并注册网关后，唯一需要的端口/IP 是 Azure 服务总线（上面的 servicebus.windows.net）所需的端口/IP。 可以通过在本地数据网关应用程序上执行[网络端口测试](../service-gateway-onprem-tshoot.md#network-ports-test)来获取所需端口的列表。

## <a name="forcing-https-communication-with-azure-service-bus"></a>强制 HTTPS 与 Azure 服务总线通信

可以使用 HTTPS 替代直接 TCP，以强制网关与 Azure 服务总线通信。

> [!NOTE]
> 从 2019 年 6 月发行版开始，根据 Azure 服务总线的建议，新装网关（非更新）默认使用 HTTPS 而不是 TCP。

若要使用 HTTPS 强制进行通信，请通过将值从 `AutoDetect` 更改为 `Https` 来修改 *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* 文件，如紧接本段后面的代码片段中所示。 默认情况下，此文件位于 *C:\Program Files\On-premises data gateway* 。

```xml
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

ServiceBusSystemConnectivityModeString  参数的值区分大小写。 有效值为 AutoDetect 和 Https   。

或者，可以使用网关用户界面来强制网关采用此行为。 在网关用户界面中，选择“网络”  ，然后将  “Azure 服务总线连接模式”切换至“启用”  。

![](./media/gateway-onprem-accounts-ports-more/gw-onprem_01.png)

更改后，在选择“应用”  （该按钮仅在做出更改时出现）后，*网关 Windows 服务* 将自动重启，从而使更改生效。

可以通过选择“服务设置”  ，然后选择“立即重启”  从用户界面对话框中重启*网关 Windows 服务* 以供将来参考之用。

![](./media/gateway-onprem-accounts-ports-more/gw-onprem_02.png)

## <a name="support-for-tls-12"></a>对 TLS 1.2 的支持

默认情况下，本地数据网关使用传输层安全 (TLS) 1.2 与 Power BI 服务进行通信。 若要确保所有网关流量使用 TLS 1.2，必须在运行网关服务的计算机上添加或修改以下注册表项：

```
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]"SchUseStrongCrypto"=dword:00000001
[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\.NETFramework\v4.0.30319]"SchUseStrongCrypto"=dword:00000001
```

> [!NOTE]
> 添加或修改这些注册表项会将更改应用于所有 .NET 应用程序。 有关影响其他应用程序的 TLS 的注册表更改的信息，请参阅[传输层安全性 (TLS) 注册表设置](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings)。

## <a name="how-to-restart-the-gateway"></a>如何重启网关

网关作为一项 Windows 服务运行。 像任何 Windows 服务一样，你可以启动和停止它。 下面是你可以如何从命令提示符执行此操作。

1. 在运行网关的计算机上，启动管理员命令提示符。
2. 使用以下命令停止服务。
   
   net stop PBIEgwService
3. 使用以下命令启动服务。
   
   net start PBIEgwService

