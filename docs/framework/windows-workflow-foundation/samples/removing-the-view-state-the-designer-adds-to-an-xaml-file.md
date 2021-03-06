---
title: 删除视图状态在设计器添加到 XAML 文件的 WF
ms.date: 03/30/2017
ms.assetid: a801ce22-8699-483c-a392-7bb3834aae4f
ms.openlocfilehash: 71a2aadeefd53b391f4589ed9dc32d9cd6e3183a
ms.sourcegitcommit: 14355b4b2fe5bcf874cac96d0a9e6376b567e4c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2019
ms.locfileid: "55260562"
---
# <a name="removing-the-view-state-the-designer-adds-to-an-xaml-file"></a>删除视图状态在设计器添加到 XAML 文件

此示例演示如何创建派生自 <xref:System.Xaml.XamlWriter> 的类以及如何从 XAML 文件中移除视图状态。 [!INCLUDE[wfd1](../../../../includes/wfd1-md.md)] 会将信息写入称作视图状态的 XAML 文档中。 视图状态是指设计时所需的信息（如布局定位），运行时不需要此信息。 [!INCLUDE[wfd2](../../../../includes/wfd2-md.md)] 将此信息插入正在编辑的 XAML 文档中。 [!INCLUDE[wfd2](../../../../includes/wfd2-md.md)] 使用 `mc:Ignorable` 特性将视图状态写入 XAML 文件中，因此在运行时加载 XAML 文件时不会加载此信息。 此示例演示如何创建一个类，此类将在处理 XAML 节点时移除视图状态信息。

## <a name="discussion"></a>讨论

此示例演示如何创建自定义编写器。

若要生成自定义 XAML 编写器，请创建一个从 <xref:System.Xaml.XamlWriter> 继承的类。 XAML 编写器经常嵌套的它是典型来跟踪的"内部"XAML 编写器。 这些"内部编写器可以看作对 XAML 编写器，您可以有多个入口点执行工作，然后将处理委托给堆栈的其余部分的剩余堆栈的引用。

在此示例中，有几个需要注意的地方。 其中一个需要注意的是，检查要编写的项是否来自某个设计器命名空间。 请注意，此操作还将消除工作流中对该设计器命名空间的其他类型的使用。

```csharp
static Boolean IsDesignerAttachedProperty(XamlMember xamlMember)
{
    return xamlMember.IsAttachable &&
        xamlMember.PreferredXamlNamespace.Equals(c_sapNamespaceURI, StringComparison.OrdinalIgnoreCase);
}

const String c_sapNamespaceURI = "http://schemas.microsoft.com/netfx/2009/xaml/activities/presentation";

// The next item of interest is the constructor, where the utilization of the inner XAML writer is seen.
public ViewStateCleaningWriter(XamlWriter innerWriter)
{
    this.InnerWriter = innerWriter;
    this.MemberStack = new Stack<XamlMember>();
}

XamlWriter InnerWriter {get; set; }
Stack<XamlMember> MemberStack {get; set; }
```

另外，此操作还将创建在遍历节点流时使用的 XAML 成员的堆栈。 此示例的剩余工作主要包含在 `WriteStartMember` 方法中。

```csharp
public override void WriteStartMember(XamlMember xamlMember)
{
    MemberStack.Push(xamlMember);

    if (IsDesignerAttachedProperty(xamlMember))
    {
        m_attachedPropertyDepth++;
    }

    if (m_attachedPropertyDepth > 0)
    {
        return;
    }

    InnerWriter.WriteStartMember(xamlMember);
}
```

然后，后续方法将检查它们是否仍包含在视图状态容器中，如果是这样，则返回且不在编写器堆栈中向下传递节点。

```csharp
public override void WriteValue(Object value)
{
    if (m_attachedPropertyDepth > 0)
    {
        return;
    }

    InnerWriter.WriteValue(value);
}
```

若要使用自定义 XAML 编写器，则必须在 XAML 编写器堆栈中将其链接起来。 下面的代码演示如何执行此操作。

```csharp
XmlWriterSettings writerSettings = new XmlWriterSettings {  Indent = true };
XmlWriter xmlWriter = XmlWriter.Create(File.OpenWrite(args[1]), writerSettings);
XamlXmlWriter xamlWriter = new XamlXmlWriter(xmlWriter, new XamlSchemaContext());
XamlServices.Save(new ViewStateCleaningWriter(ActivityXamlServices.CreateBuilderWriter(xamlWriter)), ab);
```

## <a name="to-use-this-sample"></a>使用此示例

1. 使用 Visual Studio 2010 打开 ViewStateCleaningWriter.sln 解决方案文件。

2. 打开命令提示符，导航到生成 ViewStageCleaningWriter.exe 的目录。

3. 在 Workflow1.xaml 文件上运行 ViewStateCleaningWriter.exe。

   下面的示例演示了可执行文件的语法。

   ```console
   ViewStateCleaningWriter.exe [input file] [output file]
   ```

   这将输出到一个 XAML 文件\[outfile]，其中包含所有删除其视图状态信息。

> [!NOTE]
> 已为 <xref:System.Activities.Statements.Sequence> 工作流移除大量虚拟化提示。 这将导致设计器在下次加载时重新计算布局。 在对 <xref:System.Activities.Statements.Flowchart> 使用此示例时，将移除所有定位和行路由信息，并且在后续加载到设计器中时，所有活动将在屏幕左侧堆叠。

## <a name="to-create-a-sample-xaml-file-for-use-with-this-sample"></a>创建与此示例一起使用的示例 XAML 文件

1. 打开 Visual Studio 2010。

2. 创建新的工作流控制台应用程序。

3. 将几个活动拖放到画布上。

4. 保存工作流 XAML 文件。

5. 检测 XAML 文件以查看视图状态附加属性。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请转到[Windows Communication Foundation (WCF) 和.NET Framework 4 的 Windows Workflow Foundation (WF) 示例](https://go.microsoft.com/fwlink/?LinkId=150780)若要下载所有 Windows Communication Foundation (WCF) 和[!INCLUDE[wf1](../../../../includes/wf1-md.md)]示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Designer\ViewStateCleaningWriter`