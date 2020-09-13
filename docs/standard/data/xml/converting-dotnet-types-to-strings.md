---
title: "Converting .NET Framework Types to Strings"
ms.date: "03/30/2017"
ms.technology: dotnet-standard
ms.assetid: dc2e2b65-f623-4dc3-938b-d2a054d6832c
---
# Converting .NET Framework Types to Strings
If you want to convert a .NET Framework type to a string, use the **ToString** method. The **ToString** method returns a string representation of the type passed in. The following table lists the .NET Framework types that return a string in a format that maps to the XML Schema (XSD) specifications.  
  
|.NET Framework type|String type returned|  
|-------------------------|--------------------------|  
|Boolean|"true", "false"|  
|Single.PositiveInfinity|"INF"|  
|Single.NegativeInfinity|"-INF"|  
|Double.PositiveInfinity|"INF"|  
|Double.NegativeInfinity|"-INF"|  
|DateTime|Format is yyyy-MM-ddTHH:mm:sszzzzzz and its subsets.|  
|Timespan|Format is PnYnMnTnHnMnS, for example, `P2Y10M15DT10H30M20S` is a duration of 2 years, 10 months, 15 days, 10hours, 30 minutes and 20 seconds.|  
  
## See also

- [Conversion of XML Data Types](conversion-of-xml-data-types.md)
- [Converting Strings to .NET Framework Data Types](converting-strings-to-dotnet-data-types.md)
