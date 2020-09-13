---
title: "Partitioning Data (C#)"
description: Learn how to partition data in LINQ. View an illustration showing the results of partitioning operations.
ms.date: 07/20/2015
ms.assetid: 2a5c507b-fe22-443c-a768-dec7f9ec568d
---
# Partitioning Data (C#)
Partitioning in LINQ refers to the operation of dividing an input sequence into two sections, without rearranging the elements, and then returning one of the sections.  
  
 The following illustration shows the results of three different partitioning operations on a sequence of characters. The first operation returns the first three elements in the sequence. The second operation skips the first three elements and returns the remaining elements. The third operation skips the first two elements in the sequence and returns the next three elements.  
  
 ![Illustration that shows three LINQ partitioning operations.](./media/partitioning-data/linq-partitioning-operations.png)  
  
 The standard query operator methods that partition sequences are listed in the following section.  
  
## Operators  
  
|Operator Name|Description|C# Query Expression Syntax|More Information|  
|-------------------|-----------------|---------------------------------|----------------------|  
|Skip|Skips elements up to a specified position in a sequence.|Not applicable.|<xref:System.Linq.Enumerable.Skip%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Skip%2A?displayProperty=nameWithType>|  
|SkipWhile|Skips elements based on a predicate function until an element does not satisfy the condition.|Not applicable.|<xref:System.Linq.Enumerable.SkipWhile%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.SkipWhile%2A?displayProperty=nameWithType>|  
|Take|Takes elements up to a specified position in a sequence.|Not applicable.|<xref:System.Linq.Enumerable.Take%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Take%2A?displayProperty=nameWithType>|  
|TakeWhile|Takes elements based on a predicate function until an element does not satisfy the condition.|Not applicable.|<xref:System.Linq.Enumerable.TakeWhile%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.TakeWhile%2A?displayProperty=nameWithType>|  
  
## See also

- <xref:System.Linq>
- [Standard Query Operators Overview (C#)](./standard-query-operators-overview.md)
