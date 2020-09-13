---
title: "How to serialize and deserialize JSON using C# - .NET"
description: This article shows you how to use the System.Text.Json namespace to serialize to and deserialize from JSON in .NET. It includes sample code.
ms.date: "05/13/2020"
no-loc: [System.Text.Json, Newtonsoft.Json]
helpviewer_keywords: 
  - "JSON serialization"
  - "serializing objects"
  - "serialization"
  - "objects, serializing"
---

# How to serialize and deserialize (marshal and unmarshal) JSON in .NET

This article shows how to use the <xref:System.Text.Json> namespace to serialize and deserialize to and from JavaScript Object Notation (JSON). If you're porting existing code from `Newtonsoft.Json`, see [How to migrate to `System.Text.Json`](system-text-json-migrate-from-newtonsoft-how-to.md).

The directions and sample code use the library directly, not through a framework such as [ASP.NET Core](/aspnet/core/).

Most of the serialization sample code sets <xref:System.Text.Json.JsonSerializerOptions.WriteIndented?displayProperty=nameWithType> to `true` to "pretty-print" the JSON (with indentation and whitespace for human readability). For production use, you would typically accept the default value of `false` for this setting.

The code examples refer to the following class and variants of it:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWF)]

## Namespaces

The <xref:System.Text.Json> namespace contains all the entry points and the main types. The <xref:System.Text.Json.Serialization> namespace contains attributes and APIs for advanced scenarios and customization specific to serialization and deserialization. The code examples shown in this article require `using` directives for one or both of these namespaces:

```csharp
using System.Text.Json;
using System.Text.Json.Serialization;
```

Attributes from the <xref:System.Runtime.Serialization> namespace aren't currently supported in `System.Text.Json`.

## How to write .NET objects to JSON (serialize)

To write JSON to a string or to a file, call the <xref:System.Text.Json.JsonSerializer.Serialize%2A?displayProperty=nameWithType> method.

The following example creates JSON as a string:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToString.cs?name=SnippetSerialize)]

The following example uses synchronous code to create a JSON file:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToFile.cs?name=SnippetSerialize)]

The following example uses asynchronous code to create a JSON file:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToFileAsync.cs?name=SnippetSerialize)]

The preceding examples use type inference for the type being serialized. An overload of `Serialize()` takes a generic type parameter:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToString.cs?name=SnippetSerializeWithGenericParameter)]

### Serialization example

Here's an example class that contains collections and a nested class:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithPOCOs)]

The JSON output from serializing an instance of the preceding type looks like the following example. The JSON output is minified by default:

```json
{"Date":"2019-08-01T00:00:00-07:00","TemperatureCelsius":25,"Summary":"Hot","DatesAvailable":["2019-08-01T00:00:00-07:00","2019-08-02T00:00:00-07:00"],"TemperatureRanges":{"Cold":{"High":20,"Low":-10},"Hot":{"High":60,"Low":20}},"SummaryWords":["Cool","Windy","Humid"]}
```

The following example shows the same JSON, formatted (that is, pretty-printed with whitespace and indentation):

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "DatesAvailable": [
    "2019-08-01T00:00:00-07:00",
    "2019-08-02T00:00:00-07:00"
  ],
  "TemperatureRanges": {
    "Cold": {
      "High": 20,
      "Low": -10
    },
    "Hot": {
      "High": 60,
      "Low": 20
    }
  },
  "SummaryWords": [
    "Cool",
    "Windy",
    "Humid"
  ]
}
```

### Serialize to UTF-8

To serialize to UTF-8, call the <xref:System.Text.Json.JsonSerializer.SerializeToUtf8Bytes%2A?displayProperty=nameWithType> method:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToUtf8.cs?name=SnippetSerialize)]

A <xref:System.Text.Json.JsonSerializer.Serialize%2A> overload that takes a <xref:System.Text.Json.Utf8JsonWriter> is also available.

Serializing to UTF-8 is about 5-10% faster than using the string-based methods. The difference is because the bytes (as UTF-8) don't need to be converted to strings (UTF-16).

## Serialization behavior

* By default, all public properties are serialized. You can [specify properties to exclude](#exclude-properties-from-serialization).
* The [default encoder](xref:System.Text.Encodings.Web.JavaScriptEncoder.Default) escapes non-ASCII characters, HTML-sensitive characters within the ASCII-range, and characters that must be escaped according to [the RFC 8259 JSON spec](https://tools.ietf.org/html/rfc8259#section-7).
* By default, JSON is minified. You can [pretty-print the JSON](#serialize-to-formatted-json).
* By default, casing of JSON names matches the .NET names. You can [customize JSON name casing](#customize-json-names-and-values).
* Circular references are detected and exceptions thrown.
* Currently, fields are excluded.

Supported types include:

* .NET primitives that map to JavaScript primitives, such as numeric types, strings, and Boolean.
* User-defined [Plain Old CLR Objects (POCOs)](https://stackoverflow.com/questions/250001/poco-definition).
* One-dimensional and jagged arrays (`ArrayName[][]`).
* `Dictionary<string,TValue>` where `TValue` is `object`, `JsonElement`, or a POCO.
* Collections from the following namespaces.
  * <xref:System.Collections>
  * <xref:System.Collections.Generic>
  * <xref:System.Collections.Immutable>

You can [implement custom converters](system-text-json-converters-how-to.md) to handle additional types or to provide functionality that isn't supported by the built-in converters.

## How to read JSON into .NET objects (deserialize)

To deserialize from a string or a file, call the <xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=nameWithType> method.

The following example reads JSON from a string and creates an instance of the `WeatherForecast` class shown earlier for the [serialization example](#serialization-example):

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToString.cs?name=SnippetDeserialize)]

To deserialize from a file by using synchronous code, read the file into a string, as shown in the following example:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToFile.cs?name=SnippetDeserialize)]

To deserialize from a file by using asynchronous code, call the <xref:System.Text.Json.JsonSerializer.DeserializeAsync%2A> method:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToFileAsync.cs?name=SnippetDeserialize)]

### Deserialize from UTF-8

To deserialize from UTF-8, call a <xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=nameWithType> overload that takes a `Utf8JsonReader` or a `ReadOnlySpan<byte>`, as shown in the following examples. The examples assume the JSON is in a byte array named jsonUtf8Bytes.

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToUtf8.cs?name=SnippetDeserialize1)]

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToUtf8.cs?name=SnippetDeserialize2)]

## Deserialization behavior

* By default, property name matching is case-sensitive. You can [specify case-insensitivity](#case-insensitive-property-matching).
* If the JSON contains a value for a read-only property, the value is ignored and no exception is thrown.
* Deserialization to reference types without a parameterless constructor isn't supported.
* Deserialization to immutable objects or read-only properties isn't supported.
* By default, enums are supported as numbers. You can [serialize enum names as strings](#enums-as-strings).
* Fields aren't supported.
* By default, comments or trailing commas in the JSON throw exceptions. You can [allow comments and trailing commas](#allow-comments-and-trailing-commas).
* The [default maximum depth](xref:System.Text.Json.JsonReaderOptions.MaxDepth) is 64.

You can [implement custom converters](system-text-json-converters-how-to.md) to provide functionality that isn't supported by the built-in converters.

## Serialize to formatted JSON

To pretty-print the JSON output, set <xref:System.Text.Json.JsonSerializerOptions.WriteIndented?displayProperty=nameWithType> to `true`:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripToString.cs?name=SnippetSerializePrettyPrint)]

Here's an example type to be serialized and pretty-printed JSON output:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWF)]

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot"
}
```

## Customize JSON names and values

By default, property names and dictionary keys are unchanged in the JSON output, including case. Enum values are represented as numbers. This section explains how to:

* [Customize individual property names](#customize-individual-property-names)
* [Convert all property names to camel case](#use-camel-case-for-all-json-property-names)
* [Implement a custom property naming policy](#use-a-custom-json-property-naming-policy)
* [Convert dictionary keys to camel case](#camel-case-dictionary-keys)
* [Convert enums to strings and camel case](#enums-as-strings)

For other scenarios that require special handling of JSON property names and values, you can [implement custom converters](system-text-json-converters-how-to.md).

### Customize individual property names

To set the name of individual properties, use the [[JsonPropertyName]](xref:System.Text.Json.Serialization.JsonPropertyNameAttribute) attribute.

Here's an example type to serialize and resulting JSON:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithPropertyNameAttribute)]

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "Wind": 35
}
```

The property name set by this attribute:

* Applies in both directions, for serialization and deserialization.
* Takes precedence over property naming policies.

### Use camel case for all JSON property names

To use camel case for all JSON property names, set <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy?displayProperty=nameWithType> to `JsonNamingPolicy.CamelCase`, as shown in the following example:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundTripCamelCasePropertyNames.cs?name=Serialize)]

Here's an example class to serialize and JSON output:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithPropertyNameAttribute)]

```json
{
  "date": "2019-08-01T00:00:00-07:00",
  "temperatureCelsius": 25,
  "summary": "Hot",
  "Wind": 35
}
```

The camel case property naming policy:

* Applies to serialization and deserialization.
* Is overridden by `[JsonPropertyName]` attributes. This is why the JSON property name `Wind` in the example is not camel case.

### Use a custom JSON property naming policy

To use a custom JSON property naming policy, create a class that derives from <xref:System.Text.Json.JsonNamingPolicy> and override the <xref:System.Text.Json.JsonNamingPolicy.ConvertName%2A> method, as shown in the following example:

[!code-csharp[](snippets/system-text-json-how-to/csharp/UpperCaseNamingPolicy.cs)]

Then set the <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy?displayProperty=nameWithType> property to an instance of your naming policy class:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripPropertyNamingPolicy.cs?name=SnippetSerialize)]

Here's an example class to serialize and JSON output:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithPropertyNameAttribute)]

```json
{
  "DATE": "2019-08-01T00:00:00-07:00",
  "TEMPERATURECELSIUS": 25,
  "SUMMARY": "Hot",
  "Wind": 35
}
```

The JSON property naming policy:

* Applies to serialization and deserialization.
* Is overridden by `[JsonPropertyName]` attributes. This is why the JSON property name `Wind` in the example is not upper case.

### Camel case dictionary keys

If a property of an object to be serialized is of type `Dictionary<string,TValue>`, the `string` keys can be converted to camel case. To do that, set <xref:System.Text.Json.JsonSerializerOptions.DictionaryKeyPolicy> to `JsonNamingPolicy.CamelCase`, as shown in the following example:

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeCamelCaseDictionaryKeys.cs?name=SnippetSerialize)]

Serializing an object with a dictionary named `TemperatureRanges` that has key-value pairs `"ColdMinTemp", 20` and `"HotMinTemp", 40` would result in JSON output like the following example:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "TemperatureRanges": {
    "coldMinTemp": 20,
    "hotMinTemp": 40
  }
}
```

The camel case naming policy for dictionary keys applies to serialization only. If you deserialize a dictionary, the keys will match the JSON file even if you specify `JsonNamingPolicy.CamelCase` for the `DictionaryKeyPolicy`.

### Enums as strings

By default, enums are serialized as numbers. To serialize enum names as strings, use the <xref:System.Text.Json.Serialization.JsonStringEnumConverter>.

For example, suppose you need to serialize the following class that has an enum:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithEnum)]

If the Summary is `Hot`, by default the serialized JSON has the numeric value 3:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": 3
}
```

The following sample code serializes the enum names instead of the numeric values, and converts the names to camel case:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripEnumAsString.cs?name=SnippetSerialize)]

The resulting JSON looks like the following example:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "hot"
}
```

Enum string names can be deserialized as well, as shown in the following example:

[!code-csharp[](snippets/system-text-json-how-to/csharp/RoundtripEnumAsString.cs?name=SnippetDeserialize)]

## Exclude properties from serialization

By default, all public properties are serialized. If you don't want some of them to appear in the JSON output, you have several options. This section explains how to exclude:

* [Individual properties](#exclude-individual-properties)
* [All read-only properties](#exclude-all-read-only-properties)
* [All null-value properties](#exclude-all-null-value-properties)

### Exclude individual properties

To ignore individual properties, use the [[JsonIgnore]](xref:System.Text.Json.Serialization.JsonIgnoreAttribute) attribute.

Here's an example type to serialize and JSON output:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithIgnoreAttribute)]

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
}
```

### Exclude all read-only properties

A property is read-only if it contains a public getter but not a public setter. To exclude all read-only properties, set the <xref:System.Text.Json.JsonSerializerOptions.IgnoreReadOnlyProperties?displayProperty=nameWithType> to `true`, as shown in the following example:

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeExcludeReadOnlyProperties.cs?name=SnippetSerialize)]

Here's an example type to serialize and JSON output:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithROProperty)]

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
}
```

This option applies only to serialization. During deserialization, read-only properties are ignored by default.

### Exclude all null value properties

To exclude all null value properties, set the <xref:System.Text.Json.JsonSerializerOptions.IgnoreNullValues> property to `true`, as shown in the following example:

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeExcludeNullValueProperties.cs?name=SnippetSerialize)]

Here's an example object to serialize and JSON output:

|Property |Value  |
|---------|---------|
| Date    | 8/1/2019 12:00:00 AM -07:00|
| TemperatureCelsius| 25 |
| Summary| null|

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25
}
```

This setting applies to serialization and deserialization. For information about its effect on deserialization, see [Ignore null when deserializing](#ignore-null-when-deserializing).

## Customize character encoding

By default, the serializer escapes all non-ASCII characters.  That is, it replaces them with `\uxxxx` where `xxxx` is the Unicode code of the character.  For example, if the `Summary` property is set to Cyrillic жарко, the `WeatherForecast` object is serialized as shown in this example:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "\u0436\u0430\u0440\u043A\u043E"
}
```

### Serialize language character sets

To serialize the character set(s) of one or more languages without escaping, specify [Unicode range(s)](xref:System.Text.Unicode.UnicodeRanges) when creating an instance of <xref:System.Text.Encodings.Web.JavaScriptEncoder?displayProperty=fullName>, as shown in the following example:

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeCustomEncoding.cs?name=SnippetUsings)]

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeCustomEncoding.cs?name=SnippetLanguageSets)]

This code doesn't escape Cyrillic or Greek characters. If the `Summary` property is set to Cyrillic жарко, the `WeatherForecast` object is serialized as shown in this example:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "жарко"
}
```

To serialize all language sets without escaping, use <xref:System.Text.Unicode.UnicodeRanges.All?displayProperty=nameWithType>.

### Serialize specific characters

An alternative is to specify individual characters that you want to allow through without being escaped. The following example serializes only the first two characters of жарко:

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeCustomEncoding.cs?name=SnippetUsings)]

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeCustomEncoding.cs?name=SnippetSelectedCharacters)]

Here's an example of JSON produced by the preceding code:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "жа\u0440\u043A\u043E"
}
```

### Serialize all characters

To minimize escaping you can use <xref:System.Text.Encodings.Web.JavaScriptEncoder.UnsafeRelaxedJsonEscaping?displayProperty=nameWithType>, as shown in the following example:

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeCustomEncoding.cs?name=SnippetUsings)]

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializeCustomEncoding.cs?name=SnippetUnsafeRelaxed)]

> [!CAUTION]
> Compared to the default encoder, the `UnsafeRelaxedJsonEscaping` encoder is more permissive about allowing characters to pass through unescaped:
>
> * It doesn't escape HTML-sensitive characters such as `<`, `>`, `&`, and `'`.
> * It doesn't offer any additional defense-in-depth protections against XSS or information disclosure attacks, such as those which might result from the client and server disagreeing on the *charset*.
>
> Use the unsafe encoder only when it's known that the client will be interpreting the resulting payload as UTF-8 encoded JSON. For example, you can use it if the server is sending the response header `Content-Type: application/json; charset=utf-8`. Never allow the raw `UnsafeRelaxedJsonEscaping` output to be emitted into an HTML page or a `<script>` element.

## Serialize properties of derived classes

Serialization of a polymorphic type hierarchy is not supported. For example, if a property is defined as an interface or an abstract class, only the properties defined on the interface or abstract class are serialized, even if the runtime type has additional properties. The exceptions to this behavior are explained in this section.

For example, suppose you have a `WeatherForecast` class and a derived class `WeatherForecastDerived`:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWF)]

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFDerived)]

And suppose the type argument of the `Serialize` method at compile time is `WeatherForecast`:

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializePolymorphic.cs?name=SnippetSerializeDefault)]

In this scenario, the `WindSpeed` property is not serialized even if the `weatherForecast` object is actually a `WeatherForecastDerived` object. Only the base class properties are serialized:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot"
}
```

This behavior is intended to help prevent accidental exposure of data in a derived runtime-created type.

To serialize the properties of the derived type in the preceding example, use one of the following approaches:

* Call an overload of <xref:System.Text.Json.JsonSerializer.Serialize%2A> that lets you specify the type at run time:

  [!code-csharp[](snippets/system-text-json-how-to/csharp/SerializePolymorphic.cs?name=SnippetSerializeGetType)]

* Declare the object to be serialized as `object`.

  [!code-csharp[](snippets/system-text-json-how-to/csharp/SerializePolymorphic.cs?name=SnippetSerializeObject)]

In the preceding example scenario, both approaches cause the `WindSpeed` property to be included in the JSON output:

```json
{
  "WindSpeed": 35,
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot"
}
```

> [!IMPORTANT]
> These approaches provide polymorphic serialization only for the root object to be serialized, not for properties of that root object.

You can get polymorphic serialization for lower-level objects if you define them as type `object`. For example, suppose your `WeatherForecast` class has a property named `PreviousForecast` that can be defined as type `WeatherForecast` or `object`:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithPrevious)]

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithPreviousAsObject)]

If the `PreviousForecast` property contains an instance of `WeatherForecastDerived`:

* The JSON output from serializing `WeatherForecastWithPrevious` **doesn't include** `WindSpeed`.
* The JSON output from serializing `WeatherForecastWithPreviousAsObject` **includes** `WindSpeed`.

To serialize `WeatherForecastWithPreviousAsObject`, it isn't necessary to call `Serialize<object>` or `GetType` because the root object isn't the one that may be of a derived type. The following code example doesn't call `Serialize<object>` or `GetType`:

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializePolymorphic.cs?name=SnippetSerializeSecondLevel)]

The preceding code correctly serializes `WeatherForecastWithPreviousAsObject`:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "PreviousForecast": {
    "WindSpeed": 35,
    "Date": "2019-08-01T00:00:00-07:00",
    "TemperatureCelsius": 25,
    "Summary": "Hot"
  }
}
```

The same approach of defining properties as `object` works with interfaces. Suppose you have the following interface and implementation, and you want to serialize a class with properties that contain implementation instances:

[!code-csharp[](snippets/system-text-json-how-to/csharp/IForecast.cs)]

When you serialize an instance of `Forecasts`, only `Tuesday` shows the `WindSpeed` property, because `Tuesday` is defined as `object`:

[!code-csharp[](snippets/system-text-json-how-to/csharp/SerializePolymorphic.cs?name=SnippetSerializeInterface)]

The following example shows the JSON that results from the preceding code:

```json
{
  "Monday": {
    "Date": "2020-01-06T00:00:00-08:00",
    "TemperatureCelsius": 10,
    "Summary": "Cool"
  },
  "Tuesday": {
    "Date": "2020-01-07T00:00:00-08:00",
    "TemperatureCelsius": 11,
    "Summary": "Rainy",
    "WindSpeed": 10
  }
}
```

For more information about polymorphic **serialization**, and for information about **deserialization**, see [How to migrate from Newtonsoft.Json to System.Text.Json](system-text-json-migrate-from-newtonsoft-how-to.md#polymorphic-serialization).

## Allow comments and trailing commas

By default, comments and trailing commas are not allowed in JSON. To allow comments in the JSON, set the <xref:System.Text.Json.JsonSerializerOptions.ReadCommentHandling?displayProperty=nameWithType> property to `JsonCommentHandling.Skip`.
And to allow trailing commas, set the <xref:System.Text.Json.JsonSerializerOptions.AllowTrailingCommas?displayProperty=nameWithType> property to `true`. The following example shows how to allow both:

[!code-csharp[](snippets/system-text-json-how-to/csharp/DeserializeCommasComments.cs?name=SnippetDeserialize)]

Here's example JSON with comments and a trailing comma:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25, // Fahrenheit 77
  "Summary": "Hot", /* Zharko */
}
```

## Case-insensitive property matching

By default, deserialization looks for case-sensitive property name matches between JSON and the target object properties. To change that behavior, set <xref:System.Text.Json.JsonSerializerOptions.PropertyNameCaseInsensitive?displayProperty=nameWithType> to `true`:

[!code-csharp[](snippets/system-text-json-how-to/csharp/DeserializeCaseInsensitive.cs?name=SnippetDeserialize)]

Here's example JSON with camel case property names. It can be deserialized into the following type that has Pascal case property names.

```json
{
  "date": "2019-08-01T00:00:00-07:00",
  "temperatureCelsius": 25,
  "summary": "Hot",
}
```

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWF)]

## Handle overflow JSON

While deserializing, you might receive data in the JSON that is not represented by properties of the target type. For example, suppose your target type is this:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWF)]

And the JSON to be deserialized is this:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "temperatureCelsius": 25,
  "Summary": "Hot",
  "DatesAvailable": [
    "2019-08-01T00:00:00-07:00",
    "2019-08-02T00:00:00-07:00"
  ],
  "SummaryWords": [
    "Cool",
    "Windy",
    "Humid"
  ]
}
```

If you deserialize the JSON shown into the type shown, the `DatesAvailable` and `SummaryWords` properties have nowhere to go and are lost. To capture extra data such as these properties, apply the [JsonExtensionData](xref:System.Text.Json.Serialization.JsonExtensionDataAttribute) attribute to a property of type `Dictionary<string,object>` or `Dictionary<string,JsonElement>`:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithExtensionData)]

When you deserialize the JSON shown earlier into this sample type, the extra data becomes key-value pairs of the `ExtensionData` property:

|Property |Value  |Notes  |
|---------|---------|---------|
| Date    | 8/1/2019 12:00:00 AM -07:00||
| TemperatureCelsius| 0 | Case-sensitive mismatch (`temperatureCelsius` in the JSON), so the property isn't set. |
| Summary | Hot ||
| ExtensionData | temperatureCelsius: 25 |Since the case didn't match, this JSON property is an extra and becomes a key-value pair in the dictionary.|
|| DatesAvailable:<br>  8/1/2019 12:00:00 AM -07:00<br>8/2/2019 12:00:00 AM -07:00 |Extra property from the JSON becomes a key-value pair, with an array as the value object.|
| |SummaryWords:<br>Cool<br>Windy<br>Humid |Extra property from the JSON becomes a key-value pair, with an array as the value object.|

When the target object is serialized, the extension data key value pairs become JSON properties just as they were in the incoming JSON:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 0,
  "Summary": "Hot",
  "temperatureCelsius": 25,
  "DatesAvailable": [
    "2019-08-01T00:00:00-07:00",
    "2019-08-02T00:00:00-07:00"
  ],
  "SummaryWords": [
    "Cool",
    "Windy",
    "Humid"
  ]
}
```

Notice that the `ExtensionData` property name doesn't appear in the JSON. This behavior lets the JSON make a round trip without losing any extra data that otherwise wouldn't be deserialized.

## Ignore null when deserializing

By default, if a property in JSON is null, the corresponding property in the target object is set to null. In some scenarios, the target property might have a default value, and you don't want a null value to override the default.

For example, suppose the following code represents your target object:

[!code-csharp[](snippets/system-text-json-how-to/csharp/WeatherForecast.cs?name=SnippetWFWithDefault)]

And suppose the following JSON is deserialized:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": null
}
```

After deserialization, the `Summary` property of the `WeatherForecastWithDefault` object is null.

To change this behavior, set <xref:System.Text.Json.JsonSerializerOptions.IgnoreNullValues?displayProperty=nameWithType> to `true`, as shown in the following example:

[!code-csharp[](snippets/system-text-json-how-to/csharp/DeserializeIgnoreNull.cs?name=SnippetDeserialize)]

With this option, the `Summary` property of the `WeatherForecastWithDefault` object is the default value "No summary" after deserialization.

Null values in the JSON are ignored only if they are valid. Null values for non-nullable value types cause exceptions.

## Utf8JsonReader, Utf8JsonWriter, and JsonDocument

<xref:System.Text.Json.Utf8JsonReader?displayProperty=fullName> is a high-performance, low allocation, forward-only reader for UTF-8 encoded JSON text, read from a `ReadOnlySpan<byte>` or `ReadOnlySequence<byte>`. The `Utf8JsonReader` is a low-level type that can be used to build custom parsers and deserializers. The <xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=nameWithType> method uses `Utf8JsonReader` under the covers.

<xref:System.Text.Json.Utf8JsonWriter?displayProperty=fullName> is a high-performance way to write UTF-8 encoded JSON text from common .NET types like `String`, `Int32`, and `DateTime`. The writer is a low-level type that can be used to build custom serializers. The <xref:System.Text.Json.JsonSerializer.Serialize%2A?displayProperty=nameWithType> method uses `Utf8JsonWriter` under the covers.

<xref:System.Text.Json.JsonDocument?displayProperty=fullName> provides the ability to build a read-only Document Object Model (DOM) by using `Utf8JsonReader`. The DOM provides random access to data in a JSON payload. The JSON elements that compose the payload can be accessed via the <xref:System.Text.Json.JsonElement> type. The `JsonElement` type provides array and object enumerators along with APIs to convert JSON text to common .NET types. `JsonDocument` exposes a <xref:System.Text.Json.JsonDocument.RootElement> property.

The following sections show how to use these tools for reading and writing JSON.

## Use JsonDocument for access to data

The following example shows how to use the <xref:System.Text.Json.JsonDocument> class for random access to data in a JSON string:

[!code-csharp[](snippets/system-text-json-how-to/csharp/JsonDocumentDataAccess.cs?name=SnippetAverageGrades1)]

The preceding code:

* Assumes the JSON to analyze is in a string named `jsonString`.
* Calculates an average grade for objects in a `Students` array that have a `Grade` property.
* Assigns a default grade of 70 for students who don't have a grade.
* Counts students by incrementing a `count` variable with each iteration. An alternative is to call <xref:System.Text.Json.JsonElement.GetArrayLength%2A>, as shown in the following example:

  [!code-csharp[](snippets/system-text-json-how-to/csharp/JsonDocumentDataAccess.cs?name=SnippetAverageGrades2)]

Here's an example of the JSON that this code processes:

[!code-json[](snippets/system-text-json-how-to/csharp/GradesPrettyPrint.json)]

## Use JsonDocument to write JSON

The following example shows how to write JSON from a <xref:System.Text.Json.JsonDocument>:

[!code-csharp[](snippets/system-text-json-how-to/csharp/JsonDocumentWriteJson.cs?name=SnippetSerialize)]

The preceding code:

* Reads a JSON file, loads the data into a `JsonDocument`, and writes formatted (pretty-printed) JSON to a file.
* Uses <xref:System.Text.Json.JsonDocumentOptions> to specify that comments in the input JSON are allowed but ignored.
* When finished, calls <xref:System.Text.Json.Utf8JsonWriter.Flush%2A> on the writer. An alternative is to let the writer autoflush when it's disposed.

Here's an example of JSON input to be processed by the example code:

[!code-json[](snippets/system-text-json-how-to/csharp/Grades.json)]

The result is the following pretty-printed JSON output:

[!code-json[](snippets/system-text-json-how-to/csharp/GradesPrettyPrint.json)]

## Use Utf8JsonWriter

The following example shows how to use the <xref:System.Text.Json.Utf8JsonWriter> class:

[!code-csharp[](snippets/system-text-json-how-to/csharp/Utf8WriterToStream.cs?name=SnippetSerialize)]

## Use Utf8JsonReader

The following example shows how to use the <xref:System.Text.Json.Utf8JsonReader> class:

[!code-csharp[](snippets/system-text-json-how-to/csharp/Utf8ReaderFromBytes.cs?name=SnippetDeserialize)]

The preceding code assumes that the `jsonUtf8` variable is a byte array that contains valid JSON, encoded as UTF-8.

### Filter data using Utf8JsonReader

The following example shows how to read a file synchronously and search for a value:

[!code-csharp[](snippets/system-text-json-how-to/csharp/Utf8ReaderFromFile.cs)]

The preceding code:

* Assumes the JSON contains an array of objects and each object may contain a "name" property of type string.
* Counts objects and "name" property values that end with "University".
* Assumes the file is encoded as UTF-16 and transcodes it into UTF-8. A file encoded as UTF-8 can be read directly into a `ReadOnlySpan<byte>`, by using the following code:

  ```csharp
  ReadOnlySpan<byte> jsonReadOnlySpan = File.ReadAllBytes(fileName);
  ```

  If the file contains a UTF-8 byte order mark (BOM), remove it before passing the bytes to the `Utf8JsonReader`, since the reader expects text. Otherwise, the BOM is considered invalid JSON, and the reader throws an exception.

Here's a JSON sample that the preceding code can read. The resulting summary message is "2 out of 4 have names that end with 'University'":

[!code-json[](snippets/system-text-json-how-to/csharp/Universities.json)]

### Read from a stream using Utf8JsonReader

When reading a large file (a gigabyte or more in size, for example), you might want to avoid having to load the entire file into memory at once. For this scenario, you can use a <xref:System.IO.FileStream>.

When using the `Utf8JsonReader` to read from a stream, the following rules apply:

* The buffer containing the partial JSON payload must be at least as big as the largest JSON token within it so that the reader can make forward progress.
* The buffer must be at least as big as the largest sequence of white space within the JSON.
* The reader doesn't keep track of the data it has read until it completely reads the next <xref:System.Text.Json.Utf8JsonReader.TokenType%2A> in the JSON payload. So when there are bytes left over in the buffer, you have to pass them to the reader again. You can use <xref:System.Text.Json.Utf8JsonReader.BytesConsumed%2A> to determine how many bytes are left over.

The following code illustrates how to read from a stream. The example shows a <xref:System.IO.MemoryStream>. Similar code will work with a <xref:System.IO.FileStream>, except when the `FileStream` contains a UTF-8 BOM at the start. In that case, you need to strip those three bytes from the buffer before passing the remaining bytes to the `Utf8JsonReader`. Otherwise the reader would throw an exception, since the BOM is not considered a valid part of the JSON.

The sample code starts with a 4KB buffer and doubles the buffer size each time it finds that the size is not big enough to fit a complete JSON token, which is required for the reader to make forward progress on the JSON payload. The JSON sample provided in the snippet triggers a buffer size increase only if you set a very small initial buffer size, for example, 10 bytes. If you set the initial buffer size to 10, the `Console.WriteLine` statements illustrate the cause and effect of buffer size increases. At the 4KB initial buffer size, the entire sample JSON is shown by each `Console.WriteLine`, and the buffer size never has to be increased.

[!code-csharp[](snippets/system-text-json-how-to/csharp/Utf8ReaderPartialRead.cs)]

The preceding example sets no limit to how big the buffer can grow. If the token size is too large, the code could fail with an <xref:System.OutOfMemoryException> exception. This can happen if the JSON contains a token that is around 1 GB or more in size, because doubling the 1 GB size results in a size that is too large to fit into an `int32` buffer.

## Additional resources

* [System.Text.Json overview](system-text-json-overview.md)
* [How to write custom converters](system-text-json-converters-how-to.md)
* [How to migrate from Newtonsoft.Json](system-text-json-migrate-from-newtonsoft-how-to.md)
* [DateTime and DateTimeOffset support in System.Text.Json](../datetime/system-text-json-support.md)
* [System.Text.Json API reference](xref:System.Text.Json)
<!-- * [System.Text.Json roadmap](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/roadmap/README.md)-->
