---
title: Microsoft.Transactions.TransactionBridge.PreparedMessageRetry
ms.date: 03/30/2017
ms.assetid: 2194292d-bf5f-4aef-9336-cd3c4795cb71
ms.openlocfilehash: e5735596f1b724e47cdaadd30f07629ea6b772eb
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2019
ms.locfileid: "54585924"
---
# <a name="microsofttransactionstransactionbridgepreparedmessageretry"></a>Microsoft.Transactions.TransactionBridge.PreparedMessageRetry
准备的消息重试发送到的协调程序无响应。  
  
## <a name="description"></a>描述  
 跟踪本地事务管理器在给定时间内未接收到响应时是否需要向上级协调器重新发送准备的消息/  
  
## <a name="troubleshooting"></a>疑难解答  
 调查导致响应未及时传送的潜在网络或产品问题。  如果看到很多这样的消息，可能表明基础结构有问题或响应时间异常长。 这两种问题都会明显降低系统中事务的吞吐量。  
  
## <a name="see-also"></a>请参阅
- [跟踪](../../../../../docs/framework/wcf/diagnostics/tracing/index.md)
- [使用跟踪来排除应用程序故障](../../../../../docs/framework/wcf/diagnostics/tracing/using-tracing-to-troubleshoot-your-application.md)
- [管理和诊断](../../../../../docs/framework/wcf/diagnostics/index.md)
