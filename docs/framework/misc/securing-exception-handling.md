---
title: "Securing Exception Handling"
description: See how to make exception handling secure in .NET code. Review the order in which code runs if there are try, except, catch, and finally statements.
ms.date: "03/30/2017"
dev_langs: 
  - "cpp"
helpviewer_keywords: 
  - "code security, exception handling"
  - "security [.NET Framework], exception handling"
  - "secure coding, exception handling"
  - "exception handling, security"
ms.assetid: 1f3da743-9742-47ff-96e6-d0dd1e9e1c19
---
# Securing Exception Handling

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]

In Visual C++ and Visual Basic, a filter expression further up the stack runs before any `finally` statement. The **catch** block associated with that filter runs after the `finally` statement. For more information, see [Using User-Filtered Exceptions](../../standard/exceptions/using-user-filtered-exception-handlers.md). This section examines the security implications of this order. Consider the following pseudocode example that illustrates the order in which filter statements and `finally` statements run.  
  
```cpp  
void Main()
{  
    try
    {  
        Sub();  
    }
    except (Filter())
    {  
        Console.WriteLine("catch");  
    }  
}  
bool Filter () {  
    Console.WriteLine("filter");  
    return true;  
}  
void Sub()
{  
    try
    {  
        Console.WriteLine("throw");  
        throw new Exception();  
    }
    finally
    {  
        Console.WriteLine("finally");  
    }  
}
```  
  
 This code prints the following.  
  
```output
Throw  
Filter  
Finally  
Catch  
```  
  
 The filter runs before the `finally` statement, so security issues can be introduced by anything that makes a state change where execution of other code could take advantage. For example:  
  
```cpp  
try
{  
    Alter_Security_State();  
    // This means changing anything (state variables,  
    // switching unmanaged context, impersonation, and
    // so on) that could be exploited if malicious
    // code ran before state is restored.  
    Do_some_work();  
}
finally
{  
    Restore_Security_State();  
    // This simply restores the state change above.  
}  
```  
  
 This pseudocode allows a filter higher up the stack to run arbitrary code. Other examples of operations that would have a similar effect are temporary impersonation of another identity, setting an internal flag that bypasses some security check, or changing the culture associated with the thread. The recommended solution is to introduce an exception handler to isolate the code's changes to thread state from callers' filter blocks. However, it's important to properly introduce the exception handler or this problem won't be fixed. The following example switches the UI culture, but any kind of thread state change could be similarly exposed.  
  
```cpp  
YourObject.YourMethod()  
{  
   CultureInfo saveCulture = Thread.CurrentThread.CurrentUICulture;  
   try {  
      Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");  
      // Do something that throws an exception.  
}  
   finally {  
      Thread.CurrentThread.CurrentUICulture = saveCulture;  
   }  
}  
```  
  
```vb  
Public Class UserCode  
   Public Shared Sub Main()  
      Try  
         Dim obj As YourObject = new YourObject  
         obj.YourMethod()  
      Catch e As Exception When FilterFunc  
         Console.WriteLine("An error occurred: '{0}'", e)  
         Console.WriteLine("Current Culture: {0}",
Thread.CurrentThread.CurrentUICulture)  
      End Try  
   End Sub  
  
   Public Function FilterFunc As Boolean  
      Console.WriteLine("Current Culture: {0}", Thread.CurrentThread.CurrentUICulture)  
      Return True  
   End Sub  
  
End Class  
```  
  
 The correct fix in this case is to wrap the existing **try**/**finally** block in a **try**/**catch** block. Simply introducing a **catch-throw** clause into the existing **try**/**finally** block does not fix the problem, as shown in the following example.  
  
```cpp  
YourObject.YourMethod()  
{  
    CultureInfo saveCulture = Thread.CurrentThread.CurrentUICulture;  
  
    try
    {  
        Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");  
        // Do something that throws an exception.  
    }  
    catch { throw; }  
    finally
    {  
        Thread.CurrentThread.CurrentUICulture = saveCulture;  
    }  
}  
```  
  
 This does not fix the problem because the `finally` statement has not run before the `FilterFunc` gets control.  
  
 The following example fixes the problem by ensuring that the `finally` clause has executed before offering an exception up the callers' exception filter blocks.  
  
```cpp  
YourObject.YourMethod()  
{  
    CultureInfo saveCulture = Thread.CurrentThread.CurrentUICulture;  
    try
    {  
        try
        {  
            Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");  
            // Do something that throws an exception.  
        }  
        finally
        {  
            Thread.CurrentThread.CurrentUICulture = saveCulture;  
        }  
    }  
    catch { throw; }  
}  
```  
  
## See also

- [Secure Coding Guidelines](../../standard/security/secure-coding-guidelines.md)
