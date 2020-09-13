---
title: "Directives"
ms.date: 07/20/2015
helpviewer_keywords: 
  - "directives, Visual Basic compiler"
  - "Visual Basic code, directives"
  - "directives"
ms.assetid: 20d5fe65-490a-4c23-88c2-ee4f490ed762
---
# Directives (Visual Basic)

The topics in this section document the Visual Basic source code compiler directives.  
  
## In This Section  

 [#Const Directive](const-directive.md) -- Define a compiler constant  
  
 [#ExternalSource Directive](externalsource-directive.md) -- Indicate a mapping between source lines and text external to the source  
  
 [#If...Then...#Else Directives](if-then-else-directives.md) -- Compile selected blocks of code  
  
 [#Region Directive](region-directive.md) -- Collapse and hide sections of code in the Visual Studio editor  
  
 **#Disable, #Enable** -- Disable and enable specific warnings for regions of code.  
  
```vb  
#Disable Warning BC42356 ' suppress warning about no awaits in this method  
    Async Function TestAsync() As Task  
        Console.WriteLine("testing")  
    End Function  
#Enable Warning BC42356  
```  
  
 You can disable and enable a comma-separated list of warning codes too.  
  
## Related Sections  

 [Visual Basic Language Reference](../index.md)  
  