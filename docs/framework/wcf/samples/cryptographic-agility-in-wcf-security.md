---
title: "Cryptographic agility in WCF security"
ms.date: "03/30/2017"
ms.assetid: c2c549e5-ac19-40c5-b686-8f67f52b6dbf
---
# Cryptographic agility in WCF security

This sample shows how to specify in a standard/custom algorithm to provide a cryptographic agile implementation in a Windows Communication Foundation (WCF) client and service. The sample is composed of the following projects:

**Service**

This is a self-hosted WCF service that implements the `ICalculator` interface and secures the endpoint using the <xref:System.ServiceModel.WSHttpBinding> with secure session and reliable session disabled. The service defines a custom `SecurityAlgorithmSuite` class to specify the cryptographic algorithms to be used for message security.

**Client**

This is a WCF client that accesses the service after successful authentication. It invokes the operations exposed by the `ICalculator` interface and implemented by the service. The client also defines the same custom `SecurityAlgorithmSuite` class to specify the cryptographic algorithms to be used for message security.

## To use this sample

1. Open the CryptoAgility.sln solution in Visual Studio 2012.

2. Press CTRL+SHIFT+B to build the solution.

3. Open File Explorer and navigate to the \WCF\Basic\Security\CryptoAgility\Service\bin directory and run the service.exe file with administrator privileges by right-clicking service.exe and selecting **Run as administrator**.

4. Navigate to \WCF\Basic\Security\CryptoAgility\Client\bin directory and run the client.exe file normally.

> [!IMPORTANT]
> The samples may already be installed on your machine. Check for the following (default) directory before continuing.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> If this directory does not exist, go to [Windows Communication Foundation (WCF) and Windows Workflow Foundation (WF) Samples for .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) to download all Windows Communication Foundation (WCF) and [!INCLUDE[wf1](../../../../includes/wf1-md.md)] samples. This sample is located in the following directory.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Security\CryptoAgility`

## See also

- [Windows Communication Foundation Security](../feature-details/security.md)
