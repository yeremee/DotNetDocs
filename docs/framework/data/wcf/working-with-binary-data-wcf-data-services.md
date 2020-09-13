---
title: "Working with Binary Data (WCF Data Services)"
ms.date: "03/30/2017"
dev_langs:
  - "csharp"
  - "vb"
helpviewer_keywords:
  - "WCF Data Services, binary data"
  - "WCF Data Services, streams"
ms.assetid: aeccc45c-d5c5-4671-ad63-a492ac8043ac
---
# Working with Binary Data (WCF Data Services)

The WCF Data Services client library enables you to retrieve and update binary data from an Open Data Protocol (OData) feed in one of the following ways:

- As a primitive type property of an entity. This is the recommended method for working with small binary data objects that can be easily loaded into memory. In this case, the binary property is an entity property exposed by the data model, and the data service serializes the binary data as base-64 binary encoded XML in the response message.

- As a separate binary resource stream. This is the recommended method for accessing and changing binary large object (BLOB) data that may represent a photo, video, or any other type of binary encoded data.

WCF Data Services implements the streaming of binary data by using HTTP as defined in the OData. In this mechanism, binary data is treated as a media resource that is separate from but related to an entity, which is called a media link entry. For more information, see [Streaming Provider](streaming-provider-wcf-data-services.md).

> [!TIP]
> For a step-by-step example of how to create a Windows Presentation Foundation (WPF) client application that downloads binary image files from an OData service that stores photos, see the post [Data Services Streaming Provider Series-Part 2: Accessing a Media Resource Stream from the Client](https://docs.microsoft.com/archive/blogs/astoriateam/data-services-streaming-provider-series-part-2-accessing-a-media-resource-stream-from-the-client). To download the sample code for the stream photo data service featured in the blog post, see the [Streaming Photo Data Service Sample](https://github.com/microsoftarchive/msdn-code-gallery-community-s-z/tree/master/Streaming%20Photo%20OData%20Service%20Sample) in GitHub.

## Entity Metadata

An entity that has a related media resource stream is indicated in the data service metadata by the `HasStream` attribute applied to an entity type that is the media link entry. In the following example, the `PhotoInfo` entity is a media link entry that has a related media resource, indicated by the `HasStream` attribute.

[!code-xml[Astoria Photo Streaming Service#HasStream](../../../../samples/snippets/xml/VS_Snippets_Misc/astoria_photo_streaming_service/xml/photodata.edmx#hasstream)]

The remaining examples in this topic show how to access and change the media resource stream. For a complete example of how to consume a media resource stream in a .NET Framework client application by using the WCF Data Services client library, see the post [Accessing a Media Resource Stream from the Client](https://docs.microsoft.com/archive/blogs/astoriateam/data-services-streaming-provider-series-part-2-accessing-a-media-resource-stream-from-the-client).

## Accessing the Binary Resource Stream

The WCF Data Services client library provides methods for accessing binary resource streams from an OData-based data service. When downloading a media resource, you can either use the URI of the media resource or you can get a binary stream that contains the media resource data itself. You can also upload media resource data as a binary stream.

> [!TIP]
> For a step-by-step example of how to create a Windows Presentation Foundation (WPF) client application that downloads binary image files from an OData service that stores photos, see the post [Data Services Streaming Provider Series-Part 2: Accessing a Media Resource Stream from the Client](https://docs.microsoft.com/archive/blogs/astoriateam/data-services-streaming-provider-series-part-2-accessing-a-media-resource-stream-from-the-client). To download the sample code for the stream photo data service featured in the blog post, see the [Streaming Photo Data Service Sample](https://github.com/microsoftarchive/msdn-code-gallery-community-s-z/tree/master/Streaming%20Photo%20OData%20Service%20Sample) in GitHub.

### Getting the URI of the Binary Stream

When retrieving certain types of media resources, such as images and other media files, it is often easier to use the URI of the media resource in your application than handling the binary data stream itself. To get the URI of a resource stream associated with a give media link entry, you must call the <xref:System.Data.Services.Client.DataServiceContext.GetReadStreamUri%2A> method on the <xref:System.Data.Services.Client.DataServiceContext> instance that is tracking the entity. The following example shows how to call the <xref:System.Data.Services.Client.DataServiceContext.GetReadStreamUri%2A> method to get the URI of a media resource stream that is used to create a new image on the client:

[!code-csharp[Astoria Photo Streaming Client#GetReadStreamUri](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_photo_streaming_client/cs/photowindow.xaml.cs#getreadstreamuri)]
[!code-vb[Astoria Photo Streaming Client#GetReadStreamUri](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_photo_streaming_client/vb/photowindow.xaml.vb#getreadstreamuri)]

### Downloading the Binary Resource Stream

When retrieving a binary resource stream, you must call the <xref:System.Data.Services.Client.DataServiceContext.GetReadStream%2A> method on the <xref:System.Data.Services.Client.DataServiceContext> instance that is tracking the media link entry. This method sends a request to the data service that returns a <xref:System.Data.Services.Client.DataServiceStreamResponse> object, which has a reference to the stream that contains the resource. Use this method when your application requires the binary resource as a <xref:System.IO.Stream>. The following example shows how to call the <xref:System.Data.Services.Client.DataServiceContext.GetReadStream%2A> method to retrieve a stream that is used to create a new image on the client:

[!code-csharp[Astoria Streaming Client#GetReadStreamClient](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_streaming_client/cs/customerphotowindow.xaml.cs#getreadstreamclient)]
[!code-vb[Astoria Streaming Client#GetReadStreamClient](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_streaming_client/vb/customerphotowindow.xaml.vb#getreadstreamclient)]

> [!NOTE]
> The Content-Length header in the response message that contains the binary steam is not set by the data service. This value may not reflect the actual length of the binary data stream.

### Uploading a Media Resource as a Stream

To insert or update a media resource, call the <xref:System.Data.Services.Client.DataServiceContext.SetSaveStream%2A> method on the <xref:System.Data.Services.Client.DataServiceContext> instance that is tracking the entity. This method sends a request to the data service that contains the media resource read from the supplied stream. The following example shows how to call the <xref:System.Data.Services.Client.DataServiceContext.SetSaveStream%2A> method to send an image to the data service:

[!code-csharp[Astoria Photo Streaming Client#SetSaveStream](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_photo_streaming_client/cs/photodetailswindow.xaml.cs#setsavestream)]
[!code-vb[Astoria Photo Streaming Client#SetSaveStream](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_photo_streaming_client/vb/photodetailswindow.xaml.vb#setsavestream)]

In this example, the <xref:System.Data.Services.Client.DataServiceContext.SetSaveStream%2A> method is called by supplying a value of `true` for the `closeStream` parameter. This guarantees that the <xref:System.Data.Services.Client.DataServiceContext> closes the stream after the binary data is uploaded to the data service.

> [!NOTE]
> When you call <xref:System.Data.Services.Client.DataServiceContext.SetSaveStream%2A>, the stream is not sent to the data service until <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> is called.

## See also

- [WCF Data Services Client Library](wcf-data-services-client-library.md)
- [Binding Data to Controls](binding-data-to-controls-wcf-data-services.md)
