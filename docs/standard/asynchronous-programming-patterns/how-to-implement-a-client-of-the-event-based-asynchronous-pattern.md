---
title: "How to: Implement a Client of the Event-based Asynchronous Pattern"
ms.date: "03/30/2017"
ms.technology: dotnet-standard
dev_langs: 
  - "csharp"
  - "vb"
helpviewer_keywords: 
  - "Event-based Asynchronous Pattern"
  - "ProgressChangedEventArgs class"
  - "BackgroundWorker component"
  - "events [.NET Framework], asynchronous"
  - "Asynchronous Pattern"
  - "AsyncOperationManager class"
  - "threading [.NET Framework], asynchronous features"
  - "components [.NET Framework], asynchronous"
  - "AsyncOperation class"
  - "threading [Windows Forms], asynchronous features"
  - "AsyncCompletedEventArgs class"
ms.assetid: 21a858c1-3c99-4904-86ee-0d17b49804fa
---
# How to: Implement a Client of the Event-based Asynchronous Pattern
The following code example demonstrates how to use a component that adheres to the [Event-based Asynchronous Pattern Overview](event-based-asynchronous-pattern-overview.md). The form for this example uses the `PrimeNumberCalculator` component described in [How to: Implement a Component That Supports the Event-based Asynchronous Pattern](component-that-supports-the-event-based-asynchronous-pattern.md).  
  
 When you run a project that uses this example, you will see a "Prime Number Calculator" form with a grid and two buttons: **Start New Task** and **Cancel**. You can click the **Start New Task** button several times in succession, and for each click, an asynchronous operation will begin a computation to determine if a randomly generated test number is prime. The form will periodically display progress and incremental results. Each operation is assigned a unique task ID. The result of the computation is displayed in the **Result** column; if the test number is not prime, it is labeled as **Composite,** and its first divisor is displayed.  
  
 Any pending operation can be canceled with the **Cancel** button. Multiple selections can be made.  
  
> [!NOTE]
> Most numbers will not be prime. If you have not found a prime number after several completed operations, simply start more tasks, and eventually you will find some prime numbers.  
  
## Example  
 [!code-csharp[System.ComponentModel.AsyncOperationManager#10](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#10)]
 [!code-vb[System.ComponentModel.AsyncOperationManager#10](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#10)]  
  
## See also

- <xref:System.ComponentModel.AsyncOperation>
- <xref:System.ComponentModel.AsyncOperationManager>
- <xref:System.Windows.Forms.WindowsFormsSynchronizationContext>
