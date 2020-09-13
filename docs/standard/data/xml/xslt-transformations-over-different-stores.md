---
title: "XSLT Transformations Over Different Stores"
ms.date: "03/30/2017"
ms.technology: dotnet-standard
ms.assetid: 369850e9-004a-45d2-b5c3-5060d9135adb
---
# XSLT Transformations Over Different Stores
> [!NOTE]
> The <xref:System.Xml.Xsl.XslTransform> class is obsolete in the .NET Framework 2.0. You can perform Extensible Stylesheet Language for Transformations (XSLT) transformations using the <xref:System.Xml.Xsl.XslCompiledTransform> class. See [Using the XslCompiledTransform Class](using-the-xslcompiledtransform-class.md) and [Migrating From the XslTransform Class](migrating-from-the-xsltransform-class.md) for more information.  
  
 The ADO.NET and the XML classes in the .NET Framework provide a unified programming model to access data. That data is represented as both XML data, which is text delimited by tags, and relational data, which is tables consisting of rows and columns. The XML in the .NET Framework reads XML data from any data stream into XML Document Object Model (DOM) node trees, where data can be accessed programmatically, while ADO.NET provides the means to access and manipulate relational data within a <xref:System.Data.DataSet> object.  
  
 The XML DOM provides access to data in XML documents and additional classes to read, write, and navigate in XML documents. These classes are supported in the <xref:System.Xml> namespace, which also unifies the XML DOM with the data access services provided by ADO.NET. The <xref:System.Xml.XmlDataDocument> provides relational access to data. The <xref:System.Xml.XmlDataDocument> maps XML to relational data in an ADO.NET <xref:System.Data.DataSet>. Any .NET Framework-based application can use the classes in the <xref:System.Xml> namespace to access and manipulate both XML documents and relational data in the <xref:System.Xml.XmlDataDocument>. This implementation supports n-tiered architectures for collecting and distributing data. For more information, see [XML Integration with Relational Data and ADO.NET](xml-integration-with-relational-data-and-adonet.md).  
  
## See also

- [XslTransform Class Implements the XSLT Processor](xsltransform-class-implements-the-xslt-processor.md)
