---
title: 创建 DataReader
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 49d4422a-7464-4ab8-8ec7-90185fde3ecf
ms.openlocfilehash: d3c20fa4394b09e9ceec332d430ed638166bed8e
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2019
ms.locfileid: "54491658"
---
# <a name="creating-a-datareader"></a>创建 DataReader
<xref:System.Data.DataTable> 和 <xref:System.Data.DataSet> 类具有 <xref:System.Data.DataTable.CreateDataReader%2A> 方法，可以将 <xref:System.Data.DataTable> 的内容或 <xref:System.Data.DataSet> 对象的 <xref:System.Data.DataSet.Tables%2A> 集合的内容作为一个或多个只读、只进的结果集返回。  
  
## <a name="example"></a>示例  
 以下控制台应用程序创建一个 <xref:System.Data.DataTable> 实例。 该示例然后将已填充<xref:System.Data.DataTable>到调用的过程<xref:System.Data.DataTable.CreateDataReader%2A>方法，该循环中包含的结果方法<xref:System.Data.DataTableReader>。  
  
 [!code-csharp[DataWorks DataTable.CreateDataReader#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks DataTable.CreateDataReader/CS/source.cs#1)]
 [!code-vb[DataWorks DataTable.CreateDataReader#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks DataTable.CreateDataReader/VB/source.vb#1)]  
  
 该示例在控制台窗口中显示以下输出：  
  
```  
1 Mary  
2 Andy  
3 Peter  
4 Russ  
```  
  
## <a name="see-also"></a>请参阅
- <xref:System.Data.DataTable.CreateDataReader%2A>
- <xref:System.Data.DataSet.CreateDataReader%2A>
- [DataTableReader](../../../../../docs/framework/data/adonet/dataset-datatable-dataview/datatablereaders.md)
- [ADO.NET 托管提供程序和数据集开发人员中心](https://go.microsoft.com/fwlink/?LinkId=217917)
