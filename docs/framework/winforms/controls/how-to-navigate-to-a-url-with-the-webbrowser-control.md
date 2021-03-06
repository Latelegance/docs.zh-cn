---
title: 如何：导航到使用 WebBrowser 控件的 URL
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
f1_keywords:
- WebBrowser.Navigate
helpviewer_keywords:
- WebBrowser control [Windows Forms], examples
- URLs [Windows Forms], navigating to
- WebBrowser control [Windows Forms], navigating to URLs
- examples [Windows Forms], WebBrowser control
ms.assetid: b3ec38cb-f509-4d0b-bd79-9f3611259c62
ms.openlocfilehash: 8d592aea972a95a582cc35ecb14227edec5860ce
ms.sourcegitcommit: 160a88c8087b0e63606e6e35f9bd57fa5f69c168
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2019
ms.locfileid: "57707230"
---
# <a name="how-to-navigate-to-a-url-with-the-webbrowser-control"></a>如何：导航到使用 WebBrowser 控件的 URL
下面的代码示例演示如何导航<xref:System.Windows.Forms.WebBrowser>控件到特定的 URL。  
  
 若要确定完全加载新文档时，处理<xref:System.Windows.Forms.WebBrowser.DocumentCompleted>事件。 此事件的演示，请参阅[如何：使用 WebBrowser 控件打印](how-to-print-with-a-webbrowser-control.md)。  
  
## <a name="example"></a>示例  
  
```vb  
Me.webBrowser1.Navigate("http://www.microsoft.com")  
```  
  
```csharp  
this.webBrowser1.Navigate("http://www.microsoft.com");  
```  
  
## <a name="compiling-the-code"></a>编译代码  
 此示例需要：  
  
-   名为 `webBrowser1` 的 <xref:System.Windows.Forms.WebBrowser> 控件。  
  
-   对 `System` 和 `System.Windows.Forms` 程序集的引用。  
  
## <a name="see-also"></a>请参阅
- <xref:System.Windows.Forms.WebBrowser>
- <xref:System.Windows.Forms.WebBrowser.DocumentCompleted?displayProperty=nameWithType>
- <xref:System.Windows.Forms.WebBrowser.Navigating?displayProperty=nameWithType>
- <xref:System.Windows.Forms.WebBrowser.Navigated?displayProperty=nameWithType>
- [WebBrowser 控件](webbrowser-control-windows-forms.md)
- [如何：使用 WebBrowser 控件打印](how-to-print-with-a-webbrowser-control.md)
