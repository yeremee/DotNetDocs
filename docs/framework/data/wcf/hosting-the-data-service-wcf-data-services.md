---
title: "Hosting the Data Service (WCF Data Services)"
ms.date: "03/30/2017"
dev_langs:
  - "csharp"
  - "vb"
helpviewer_keywords:
  - "WCF Data Services, configuring"
  - "WCF Data Services, Windows Communication Foundation"
ms.assetid: b48f42ce-22ce-4f8d-8f0d-f7ddac9125ee
---
# Hosting the Data Service (WCF Data Services)
By using WCF Data Services, you can create a service that exposes data as an Open Data Protocol (OData) feed. This data service is defined as a class that inherits from <xref:System.Data.Services.DataService%601>. This class provides the functionality required to process request messages, perform updates against the data source, and generate responses messages, as required by OData. However, a data service cannot bind to and listen on a network socket for incoming HTTP requests. For this required functionality, the data service relies on a hosting component.

 The data service host performs the following tasks on behalf of the data service:

- Listens for requests and routes these requests to the data service.

- Creates an instance of the data service for each request.

- Requests that the data service process the incoming request.

- Sends the response on behalf of the data service.

 To simplify hosting a data service, WCF Data Services is designed to integrate with Windows Communication Foundation (WCF). The data service provides a default WCF implementation that serves as the data service host in an ASP.NET application. Therefore, you can host a data service in one of the following ways:

- In an ASP.NET application.

- In a managed application that supports self-hosted WCF services.

- In some other custom data service host.

## Hosting a Data Service in an ASP.NET Application

When you use the **Add New Item** dialog in Visual Studio 2015 to define a data service in an ASP.NET application, the tool generates two new files in the project. The first file has a `.svc` extension and instructs the WCF runtime how to instantiate the data service. The following is an example of this file for the Northwind sample data service created when you complete the [quickstart](quickstart-wcf-data-services.md):

```aspx-csharp
<%@ ServiceHost Language="C#"
    Factory="System.Data.Services.DataServiceHostFactory,
            System.Data.Services, Version=4.0.0.0,
            Culture=neutral, PublicKeyToken=b77a5c561934e089"
    Service="NorthwindService.Northwind" %>
```

 This directive tells the application to create the service host for the named data service class by using the <xref:System.Data.Services.DataServiceHostFactory> class.

 The code-behind page for the `.svc` file contains the class that is the implementation of the data service itself, which is defined as follows for the Northwind sample data service:

 [!code-csharp[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_service/cs/northwind.svc.cs#servicedefinition)]
 [!code-vb[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_service/vb/northwind.svc.vb#servicedefinition)]

 Because a data service behaves like a WCF service, the data service integrates with ASP.NET and follows the WCF Web programming model. For more information, see [WCF Services and ASP.NET](../../wcf/feature-details/wcf-services-and-aspnet.md) and [WCF Web HTTP Programming Model](../../wcf/feature-details/wcf-web-http-programming-model.md).

## Self-Hosted WCF Services
 Because it incorporates a WCF implementation, WCF Data Services support self-hosting a data service as a WCF service. A service can be self-hosted in any .NET Framework application, such as a console application. The <xref:System.Data.Services.DataServiceHost> class, which inherits from <xref:System.ServiceModel.Web.WebServiceHost>, is used to instantiate the data service at a specific address.

 Self-hosting can be used for development and testing because it can make it easier to deploy and troubleshoot the service. However, this kind of hosting does not provide the advanced hosting and management features provided by ASP.NET or by Internet Information Services (IIS). For more information, see [Hosting in a Managed Application](../../wcf/feature-details/hosting-in-a-managed-application.md).

## Defining a Custom Data Service Host
 For cases where the WCF host implementation is too restrictive, you can also define a custom host for a data service. Any class that implements <xref:System.Data.Services.IDataServiceHost> interface can be used as the network host for a data service. A custom host must implement the <xref:System.Data.Services.IDataServiceHost> interface and be able to handle the following basic responsibilities of the data service host:

- Provide the data service with the service root path.

- Process request and response headers information to the appropriate <xref:System.Data.Services.IDataServiceHost> member implementation.

- Handle exceptions raised by the data service.

- Validate parameters in the query string.

## See also

- [Defining WCF Data Services](defining-wcf-data-services.md)
- [Exposing Your Data as a Service](exposing-your-data-as-a-service-wcf-data-services.md)
- [Configuring the Data Service](configuring-the-data-service-wcf-data-services.md)
