---
title: "XmlDocument Input to XslTransform"
ms.date: "03/30/2017"
ms.technology: dotnet-standard
dev_langs: 
  - "csharp"
  - "vb"
ms.assetid: 97115892-410a-4657-ab47-1e14dfba73f8
---
# XmlDocument Input to XslTransform
The <xref:System.Xml.XmlDocument> class provides editing capabilities for an XML document. If the XML needs to be edited or modified before being sent to the <xref:System.Xml.Xsl.XslTransform.Transform%2A> method, load the XML into an <xref:System.Xml.XmlDocument>, edit it, and send it in to the <xref:System.Xml.Xsl.XslTransform>.  
  
> [!NOTE]
> The <xref:System.Xml.Xsl.XslTransform> class is obsolete in the .NET Framework 2.0. You can perform Extensible Stylesheet Language for Transformations (XSLT) transformations using the <xref:System.Xml.Xsl.XslCompiledTransform> class. See [Using the XslCompiledTransform Class](using-the-xslcompiledtransform-class.md) and [Migrating From the XslTransform Class](migrating-from-the-xsltransform-class.md) for more information.  
  
 The <xref:System.Xml.XmlDocument> implements the <xref:System.Xml.XPath.IXPathNavigable> interface, so the document can be passed to the <xref:System.Xml.Xsl.XslTransform.Transform%2A> method after editing.  
  
 Due to the editing capability of the <xref:System.Xml.XmlDocument>, using the <xref:System.Xml.XmlDocument> class as input to a transformation is slower than using an <xref:System.Xml.XPath.XPathDocument> for the Extensible Stylesheet Language for Transformations (XSLT) transformations, as the <xref:System.Xml.XPath.XPathDocument> is optimized for XML Path Language (XPath) queries due to the internal storage.  
  
## Example  
 The following code example shows how an <xref:System.Xml.XmlDocument> can be supplied to the <xref:System.Xml.Xsl.XslTransform>, with the output sent to an <xref:System.Xml.XmlReader>.  
  
```vb  
Dim doc as XmlDocument = new XmlDocument()  
doc.Load("books.xml")  
Dim trans As XslTransform = new XslTransform()  
trans.Load("book.xsl")  
Dim rdr As XmlReader = trans.Transform(doc, Nothing, Nothing)  
while (rdr.Read())  
end while  
```  
  
```csharp  
XmlDocument doc = new XmlDocument();  
doc.Load("books.xml");  
XslTransform trans = new XslTransform();  
trans.Load("book.xsl");  
XmlReader rdr = trans.Transform(doc, null, null);  
while (rdr.Read()) {}  
```  
  
## See also

- <xref:System.Xml.XmlDocument>
- [XSLT Transformations with the XslTransform Class](xslt-transformations-with-the-xsltransform-class.md)
- [XslTransform Class Implements the XSLT Processor](xsltransform-class-implements-the-xslt-processor.md)
- [XPathNavigator in Transformations](xpathnavigator-in-transformations.md)
- [XPathNodeIterator in Transformations](xpathnodeiterator-in-transformations.md)
- [XPathDocument Input to XslTransform](xpathdocument-input-to-xsltransform.md)
- [XmlDataDocument Input to XslTransform](xmldatadocument-input-to-xsltransform.md)
