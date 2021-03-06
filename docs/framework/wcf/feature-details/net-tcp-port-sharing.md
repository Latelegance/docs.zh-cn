---
title: Net.TCP 端口共享
ms.date: 03/30/2017
helpviewer_keywords:
- port activation [WCF]
- port sharing [WCF]
ms.assetid: f13692ee-a179-4439-ae72-50db9534eded
ms.openlocfilehash: e191dc62368fc9c16bd58efd30dd1a3769d2bb88
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2019
ms.locfileid: "54540307"
---
# <a name="nettcp-port-sharing"></a>Net.TCP 端口共享
Windows Communication Foundation (WCF) 提供高性能通信的新的基于 TCP 的网络协议 (net.tcp://)。 WCF 还引入了新的系统组件使得 net.tcp 端口可以在多个用户进程之间共享 Net.TCP 端口共享服务。  
  
## <a name="background-and-motivation"></a>背景和动机  
 刚开始引入 TCP/IP 协议时，只有少量应用程序协议使用它。 TCP/IP 通过为每一个应用程序协议分配一个唯一的 16 位端口号，从而使用端口号来区分应用程序。 例如，HTTP 通信现在已经统一为使用 TCP 端口 80，SMTP 使用 TCP 端口 25，FTP 使用 TCP 端口 20 和 21。 其他使用 TCP 作为传输协议的应用程序可以按习惯或遵循正式标准选择其他可用的端口号。  
  
 可使用端口号区分存在安全问题的应用程序。 除少数几个已知的入口点之外，防火墙通常会配置为阻塞 TCP 通信，因此，部署使用非标准端口的应用程序经常会因为存在公司防火墙和个人防火墙而变得复杂或者甚至无法实现。 通过已得到允许的标准已知端口进行通信的应用程序可减少外部攻击面。 许多网络应用程序使用 HTTP 协议，这是因为大多数防火墙在默认情况下会配置为允许 TCP 端口 80 上的通信。  
  
 在 HTTP.SYS 模型中，许多不同的 HTTP 应用程序的通信中将多路复用到单个 TCP 端口。此模型已经成为 Windows 平台上的标准。 这为防火墙管理员提供了一个公共控制点，同时可以让应用程序开发人员尽可能降低生成可利用网络的新应用程序的部署成本。  
  
 在多个 HTTP 应用程序之间共享端口的能力早已成为 Internet 信息服务 (IIS) 的一个功能。 但是，只有在引入 [!INCLUDE[iis601](../../../../includes/iis601-md.md)] 附带的 HTTP.SYS（核心模式 HTTP 协议侦听器）之后，此基础结构才完全得到广泛使用。 实际上，HTTP.SYS 允许任意用户进程共享专用于 HTTP 通信的 TCP 端口。 此功能可以让许多 HTTP 应用程序在同一台计算机上共存于不同的独立进程中，同时共享通过 TCP 端口 80 发送和接收通信所需要的网络基础结构。 Net.TCP Port Sharing Service 支持为 net.tcp 应用程序共享相同类型的端口。  
  
## <a name="port-sharing-architecture"></a>端口共享结构  
 WCF 中的端口共享结构有三个主要组件：  
  
-   辅助进程：通过使用共享的端口 net.tcp:// 进行通信的任何进程。  
  
-   WCF TCP 传输：实现 net.tcp:// 协议。  
  
-   Net.TCP 端口共享服务：允许许多辅助进程共享同一 TCP 端口。  
  
 Net.TCP Port Sharing Service 是用户模式的 Windows 服务，可代表通过其连接的辅助进程接受 net.tcp:// 连接。 当套接字连接到达时，端口共享服务将检查传入消息流以获取其目标地址。 基于此地址，端口共享服务可以将数据流路由到最终处理它的应用程序。  
  
 当使用 net.tcp:// 端口共享打开的 WCF 服务时，WCF TCP 传输协议基础结构不会直接打开 TCP 套接字应用程序进程中。 传输协议基础结构而是向 Net.TCP Port Sharing Service 注册服务的基址统一资源标识符 (URI)，并且等待端口共享服务代表它侦听消息。  端口共享服务将对到达的发送给该应用程序服务的消息进行调度。  
  
## <a name="installing-port-sharing"></a>安装端口共享  
 Net.TCP Port Sharing Service 可用于支持 [!INCLUDE[vstecwinfx](../../../../includes/vstecwinfx-md.md)] 的所有操作系统，但是默认情况下不启用该服务。 作为一项安全预防措施，管理员必须在第一次使用前手动启用 Net.TCP Port Sharing Service。 Net.TCP Port Sharing Service 会公开一些配置选项，从而让您可以操作端口共享服务所拥有的网络套接字的若干特性。 有关详细信息，请参阅[如何：启用 Net.TCP 端口共享服务](../../../../docs/framework/wcf/feature-details/how-to-enable-the-net-tcp-port-sharing-service.md)。  
  
## <a name="using-nettcp-port-sharing-in-an-application"></a>在应用程序中使用 Net.tcp 端口共享  
 若要使用 net.tcp:// 端口在 WCF 应用程序中共享的最简单方法是公开服务使用<xref:System.ServiceModel.NetTcpBinding>，然后启用 Net.TCP Port Sharing Service 使用<xref:System.ServiceModel.NetTcpBinding.PortSharingEnabled%2A>属性。  
  
 有关如何执行此操作的详细信息，请参阅[如何：配置 WCF 服务以使用端口共享](../../../../docs/framework/wcf/feature-details/how-to-configure-a-wcf-service-to-use-port-sharing.md)。  
  
## <a name="security-implications-of-port-sharing"></a>端口共享的安全性影响  
 虽然 Net.TCP Port Sharing Service 在应用程序和网络之间提供了一个处理层，但是仍应对使用端口共享的应用程序进行保护，就好像这些应用程序直接在网络上进行侦听一样。 具体来说，使用端口共享的应用程序应评估运行它们所依据的进程特权。 请考虑使用内置网络服务帐户运行应用程序，该内置服务帐户将以网络通信要求的最小进程特权集运行。  
  
## <a name="see-also"></a>请参阅
- [配置 Net.TCP 端口共享服务](../../../../docs/framework/wcf/feature-details/configuring-the-net-tcp-port-sharing-service.md)
- [承载](../../../../docs/framework/wcf/feature-details/hosting.md)
- [如何：配置 WCF 服务以使用端口共享](../../../../docs/framework/wcf/feature-details/how-to-configure-a-wcf-service-to-use-port-sharing.md)
- [如何：启用 Net.TCP 端口共享服务](../../../../docs/framework/wcf/feature-details/how-to-enable-the-net-tcp-port-sharing-service.md)
