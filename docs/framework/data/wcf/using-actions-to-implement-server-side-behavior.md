---
title: "Using actions to implement server-side behavior"
ms.date: "03/30/2017"
ms.assetid: 11a372db-7168-498b-80d2-9419ff557ba5
---
# Using actions to implement server-side behavior

OData Actions provide a way to implement a behavior that acts upon a resource retrieved from an OData service. For example consider a digital movie as a resource, there are many things you may do with a digital movie: check-out, rate/comment, or check-in. These are all examples of Actions that may be implemented by a WCF Data Service that manages digital movies. Actions are described in an OData response that contains a resource on which the Action can be invoked. When a user requests a resource that represents a digital movie the response returned from the WCF Data Service contains information about the Actions that are available for that resource. The availability of an Action can depend on the state of the data service or resource. For example once a digital movie is checked out it cannot be checked out by another user. Clients can invoke an action simply by specifying a URL. For example, `http://MyServer/MovieService.svc/Movies(6)` would identify a specific digital movie and `http://MyServer/MovieService.svc/Movies(6)/Checkout` would invoke the action on the specific movie. Actions allow you to expose you service model without exposing your data model. Continuing with the movie service example, you may wish to allow a user to rate a movie, but not directly expose the rating data as a resource. You could implement a Rate Action to allow the user to rate a movie but not directly access the rating data as a resource.

## Implementing an action
 To implement a service action you must implement the <xref:System.IServiceProvider>, [IDataServiceActionProvider](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859915(v=vs.103)), and [IDataServiceInvokable](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859893(v=vs.103)) interfaces. <xref:System.IServiceProvider> allows WCF Data Services to get your implementation of [IDataServiceActionProvider](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859915(v=vs.103)). [IDataServiceActionProvider](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859915(v=vs.103)) allows WCF Data Services to create, find, describe and invoke service actions. [IDataServiceInvokable](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859893(v=vs.103)) allows you to invoke the code that implements the service actions’ behavior and get the results, if any. Keep in mind that WCF Data Services are Per-Call WCF Services, a new instance of the service will be created each time the service is called.  Make sure no unnecessary work is done when the service is created.

### IServiceProvider
 <xref:System.IServiceProvider> contains a method called <xref:System.IServiceProvider.GetService%2A>. This method is called by WCF Data Services to retrieve a number of service providers, including metadata service providers and data service action providers. When asked for a data service action provider, return your [IDataServiceActionProvider](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859915(v=vs.103)) implementation.

### IDataServiceActionProvider
 [IDataServiceActionProvider](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859915(v=vs.103)) contains methods that allow you to retrieve information about the available actions. When you implement [IDataServiceActionProvider](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859915(v=vs.103)) you are augmenting the metadata for your service which is defined by your service’s implementation of [IDataServiceActionProvider](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859915(v=vs.103)) with Actions and handling dispatch to those actions as appropriate.

#### AdvertiseServiceAction
 The [AdvertiseServiceAction method](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859971(v=vs.103)) is called to determine what actions are available for the specified resource. This method is only called for actions that are not always available. It is used to check if the action should be included in the OData response based upon the state of the resource being requested or the state of the service. How this check is accomplished is completely up to you. If it is an expensive to calculate availability and the current resource is in a feed, it is acceptable to skip the check and advertise the action. The `inFeed` parameter is set to `true` if the current resource being returned is part of a feed.

#### CreateInvokable
 [CreateInvokable](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859940(v=vs.103)) is called to create a [IDataServiceInvokable](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859893(v=vs.103)) that contains a delegate that encapsulates the code that implements the action’s behavior. This creates the [IDataServiceInvokable](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859893(v=vs.103)) instance but does not invoke the action. WCF Data Service Actions have side effects and need to work in conjunction with the Update Provider to save those changes to disk. The [IDataServiceInvokable.Invoke](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859924(v=vs.103)) method is called from the Update Provider’s SaveChanges() method is called.

#### GetServiceActions
 This method returns a collection of [ServiceAction](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh544089(v=vs.103)) instances that represent all of the actions a WCF Data Service exposes. [ServiceAction](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh544089(v=vs.103)) is the metadata representation of an Action and includes information like the Action name, its parameters, and its return type.

#### GetServiceActionsByBindingParameterType
 This method returns a collection of all [ServiceAction](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh544089(v=vs.103)) instances that can be bound to the specified binding parameter type. In other words, all [ServiceAction](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh544089(v=vs.103))s that can act on the specified resource type (also called binding parameter type).This is used when the service returns a resource in order to include information about Actions that can be invoked against that resource. This method should only return actions that can bind to the exact binding parameter type (no derived types). This method is called once per request per type encountered and the result is cached by WCF Data Services.

#### TryResolveServiceAction
 This method searches for a specified [ServiceAction](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh544089(v=vs.103)) and returns `true` if the [ServiceAction](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh544089(v=vs.103)) is found. If found, the [ServiceAction](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh544089(v=vs.103)) is returned in the `serviceAction` `out` parameter.

### IDataServiceInvokable
 This interface provides a way to execute a WCF Data Service Action. When implementing IDataServiceInvokable you are responsible for 3 things:

1. Capturing and potentially marshaling the parameters

2. Dispatching the parameters to the code that actually implements the Action when Invoke() is called

3. Storing any results from Invoke() so they can be retrieved using GetResult()

 The parameters may be passed as tokens. This is because it is possible to write a Data Service Provider that works with tokens that represent resources, if this is the case you may need to convert (marshal) these tokens into actual resources before dispatching to the actual action. After the parameter has been marshaled, it must be in an editable state so that any changes to the resource that occur when the action is invoked will be saved and written to disk.

 This interface requires two methods: Invoke and GetResult. Invoke invokes the delegate that implements the action’s behavior and GetResult returns the result of the action.

## Invoking a WCF Data Service Action
 Actions are invoked using an HTTP POST request. The URL specifies the resource followed by the action name. Parameters are passed in the body of the request. For example if there was a service called MovieService which exposed an action called Rate. You could use the following URL to invoke the Rate action on a specific movie:

 `http://MovieServer/MovieService.svc/Movies(1)/Rate`

 Movies(1) specifies the movie you wish to rate and Rate specifies the Rate action. The actual value of the rating will be in the body of the HTTP request as shown in the following example:

```http
POST http://MovieServer/MoviesService.svc/Movies(1)/Rate HTTP/1.1
Content-Type: application/json
Content-Length: 20
Host: localhost:15238
{
   "rating": 4
}
```

> [!WARNING]
> The above sample code will work only with WCF Data Services 5.2 and later which has support for JSON light. If using an earlier version of WCF Data Services, you must specify the json verbose content-type as follows: `application/json;odata=verbose`.

 Alternatively you can invoke an action using the WCF Data Services Client as shown in the following code snippet.

```csharp
MoviesModel context = new MoviesModel (new Uri("http://MyServer/MoviesService.svc/"));
//...
context.Execute(new Uri("http://MyServer/MoviesService.svc/Movies(1)/Rate"), "POST", new BodyOperationParameter("rating",4) );
```

 In the code snippet above, the `MoviesModel` class was generated by using Visual Studio to Add Service Reference to a WCF Data Service.

## See also

- [WCF Data Services 4.5](index.md)
- [Defining WCF Data Services](defining-wcf-data-services.md)
- [Developing and Deploying WCF Data Services](developing-and-deploying-wcf-data-services.md)
- [Custom Data Service Providers](custom-data-service-providers-wcf-data-services.md)
