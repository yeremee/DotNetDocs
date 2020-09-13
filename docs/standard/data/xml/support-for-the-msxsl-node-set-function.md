---
title: "Support for the msxsl:node-set() Function"
ms.date: "03/30/2017"
ms.technology: dotnet-standard
ms.assetid: d0cbf517-d9f6-4097-9851-4fa62903decd
---
# Support for the msxsl:node-set() Function
The `msxsl:node-set` function enables you to convert a result tree fragment into a node set. The resulting node set always contains a single node and is the root node of the tree.  
  
> [!NOTE]
> The <xref:System.Xml.Xsl.XslTransform> class is obsolete in the .NET Framework 2.0. You can perform Extensible Stylesheet Language for Transformations (XSLT) transformations using the <xref:System.Xml.Xsl.XslCompiledTransform> class. See [Using the XslCompiledTransform Class](using-the-xslcompiledtransform-class.md) and [Migrating From the XslTransform Class](migrating-from-the-xsltransform-class.md) for more information.  
  
 The `msxsl:node-set` function enables you to convert a result tree fragment into a node set. The resulting node set always contains a single node and is the root node of the tree.  
  
## Example  
 In the following example, `$var` is a variable that is a node tree in the style sheet. The for-each statement combined with the `node-set` function allows the user to iterate over this node tree as a node set.  
  
## nodeset.xsl  
  
```xml  
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform"  
                xmlns:msxsl="urn:schemas-microsoft-com:xslt"  
                xmlns:user="http://www.contoso.com"  
                version="1.0">  
    <xsl:variable name="books">  
        <book author="Michael Howard">Writing Secure Code</book>  
        <book author="Michael Kay">XSLT Reference</book>  
    </xsl:variable>  
  
    <xsl:template match="/">  
        <authors>  
            <xsl:for-each select="msxsl:node-set($books)/book">
                <author><xsl:value-of select="@author"/></author>  
            </xsl:for-each>  
        </authors>  
    </xsl:template>  
</xsl:stylesheet>  
```  
  
## Output  
 The output of the transformation is  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<authors><author>Michael Howard</author><author>Michael Kay</author></authors>  
```  
  
## See also

- [XslTransform Class Implements the XSLT Processor](xsltransform-class-implements-the-xslt-processor.md)
