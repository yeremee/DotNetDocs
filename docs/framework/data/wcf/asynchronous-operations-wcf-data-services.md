---
title: "Asynchronous Operations (WCF Data Services)"
ms.date: "03/30/2017"
helpviewer_keywords: 
  - "WCF Data Services, asynchronous operations"
  - "asynchronous operations [WCF Data Services]"
  - "WCF Data Services, client library"
ms.assetid: 679644c7-e3fc-422c-b14a-b44b683900d0
---
# Asynchronous Operations (WCF Data Services)
Web applications must accommodate higher latency between client and server than applications that run inside internal networks. To optimize the performance and user experience of your application, we recommend using the asynchronous methods of the <xref:System.Data.Services.Client.DataServiceContext> and <xref:System.Data.Services.Client.DataServiceQuery%601> classes when accessing WCF Data Services servers over the Web.  
  
 Although the WCF Data Services servers process HTTP requests asynchronously, some methods of the WCF Data Services client libraries are synchronous and wait until the entire request-response exchange is completed before continuing execution. The asynchronous methods of the WCF Data Services client libraries do not wait for this exchange to complete and can allow your application to maintain a responsive user interface in the meantime.  
  
 You can perform asynchronous operations by using a pair of methods on the <xref:System.Data.Services.Client.DataServiceContext> and <xref:System.Data.Services.Client.DataServiceQuery%601> classes that start with *Begin* and *End* respectively. The *Begin* methods register a delegate that the service calls when the operation is complete. The *End* methods should be called in the delegate that is registered to handle the callback from the completed operations. When you call the *End* method to complete an asynchronous operation, you must do so from the same <xref:System.Data.Services.Client.DataServiceQuery%601> or <xref:System.Data.Services.Client.DataServiceContext> instance that you used to begin the operation. Each *Begin* method takes a `state` parameter that can pass a state object to the callback. This state object is retrieved from the <xref:System.IAsyncResult> that is supplied with the callback and is used to call the corresponding *End* method to complete the asynchronous operation. For example, when you supply the <xref:System.Data.Services.Client.DataServiceQuery%601> instance as the `state` parameter when you call the <xref:System.Data.Services.Client.DataServiceQuery%601.BeginExecute%2A> method on the instance, the same <xref:System.Data.Services.Client.DataServiceQuery%601> instance is returned by the <xref:System.IAsyncResult>. This instance of <xref:System.Data.Services.Client.DataServiceQuery%601> is then used to call the <xref:System.Data.Services.Client.DataServiceQuery%601.EndExecute%2A> method to complete the query operation. For more information, see [How to: Execute Asynchronous Data Service Queries](how-to-execute-asynchronous-data-service-queries-wcf-data-services.md).  
  
> [!NOTE]
> Only asynchronous operations are supported by the client libraries that are provided in the .NET Framework for Silverlight. For more information, see [WCF Data Services (Silverlight)](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838234(v=vs.95)).  
  
 The .NET Framework client libraries support the following asynchronous operations:  
  
|Operation|Methods|  
|---------------|-------------|  
|Executing a <xref:System.Data.Services.Client.DataServiceQuery%601>.|-   <xref:System.Data.Services.Client.DataServiceQuery%601.BeginExecute%2A><br />-   <xref:System.Data.Services.Client.DataServiceQuery%601.EndExecute%2A>|  
|Executing a query from the <xref:System.Data.Services.Client.DataServiceContext>.|-   <xref:System.Data.Services.Client.DataServiceContext.BeginExecute%2A><br />-   <xref:System.Data.Services.Client.DataServiceContext.EndExecute%2A>|  
|Executing a batch query from the <xref:System.Data.Services.Client.DataServiceContext>.|-   <xref:System.Data.Services.Client.DataServiceContext.BeginExecuteBatch%2A><br />-   <xref:System.Data.Services.Client.DataServiceContext.EndExecuteBatch%2A>|  
|Loading a related entity into the <xref:System.Data.Services.Client.DataServiceContext>.|-   <xref:System.Data.Services.Client.DataServiceContext.BeginLoadProperty%2A><br />-   <xref:System.Data.Services.Client.DataServiceContext.EndLoadProperty%2A>|  
|Saving changes to objects in the <xref:System.Data.Services.Client.DataServiceContext>|-   <xref:System.Data.Services.Client.DataServiceContext.BeginSaveChanges%2A><br />-   <xref:System.Data.Services.Client.DataServiceContext.EndSaveChanges%2A>|  
  
## Threading Considerations for Asynchronous Operations  
 In a multi-threaded application, the delegate that is registered as a callback for the asynchronous operation is not necessarily invoked on the same thread that was used to call the *Begin* method, which creates the initial request. In an application where the callback must be invoked on a specific thread, you must explicitly marshal the execution of the *End* method, which handles the response, to the desired thread. For example, in Windows Presentation Foundation (WPF)-based applications and Silverlight-based applications, the response must be marshaled back to the UI thread by using the <xref:System.Windows.Threading.Dispatcher.BeginInvoke%2A> method on the <xref:System.Windows.Threading.Dispatcher> object. For more information, see [Querying the Data Service (WCF Data Services/Silverlight)](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc903932(v=vs.95)).  
  
## See also

- [WCF Data Services Client Library](wcf-data-services-client-library.md)
