---
title: "UriTemplate Table Dispatcher Sample"
ms.date: "03/30/2017"
ms.assetid: 3b32975d-ba90-4c5c-83bc-2fbb48f11c0c
---
# UriTemplate Table Dispatcher Sample
The <xref:System.UriTemplateTable> class provides a dictionary-like associative table structure for working with a set of <xref:System.UriTemplate> instances. This sample demonstrates a basic dispatching engine built using `UriTemplateTable`, a common usage scenario for the `UriTemplateTable` class.  
  
 This sample demonstrates the following key concepts for the `UriTemplateTable` class:  
  
- Associating delegates with `UriTemplates` in a `UriTemplateTable`.  
  
- Using <xref:System.UriTemplateTable.MatchSingle%2A> to obtain the correct handler delegate for a particular URI.  
  
- Invoking the handler delegate to process the request.  
  
### To set up, build, and run the sample  
  
1. To build the C# or Visual Basic .NET edition of the solution, follow the instructions in [Building the Windows Communication Foundation Samples](building-the-samples.md).  
  
2. To run the sample in a single- or cross-machine configuration, follow the instructions in [Running the Windows Communication Foundation Samples](running-the-samples.md).  
  
> [!IMPORTANT]
> The samples may already be installed on your machine. Check for the following (default) directory before continuing.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> If this directory does not exist, go to [Windows Communication Foundation (WCF) and Windows Workflow Foundation (WF) Samples for .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) to download all Windows Communication Foundation (WCF) and [!INCLUDE[wf1](../../../../includes/wf1-md.md)] samples. This sample is located in the following directory.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Web\UriTemplateDispatcher`  
  
## See also

- [UriTemplate Table](uritemplate-table-sample.md)
- [UriTemplate](uritemplate-sample.md)
