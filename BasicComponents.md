## `Dodoni.BasicComponents`

#### 1. Overview
This assembly contains basic classes and methods needed for the Dodoni.net project. This assembly provide general stuff which is not directly related to mathematical problems, 
mathematical finance, Excel interface etc.

#### 2. Dependencies
This assembly does not depend on any other assembly of the Dodoni.net project. Instead each assembly of the Dodoni.net project depends on `Dodoni.BasicComponents`.

#### 3. Main concepts and helpful code snippets
One may have a look into the unit test project and integration test project of 
`Dodoni.BasicComponents`; see API documentation for more information.

 **IInfoOutputQueriable**
It is well-known that the `ToString` method can be used to return a string representation of a specific object. In many cases it would be nice to have more than a string representation only, for example a collection of property names and property values etc. That is the reason why the interface `IInfoOutputQueriable` has been established. With this interface it is possible to fetch a collection of properties, DataTable objects etc. from a specific instance. The values are used for some kind of output only (as in the case of the `ToString()` method), therefore the output is always read-only and often not type safe. Moreover the output can be grouped in some _package_. This interface is heavily used and allows in a simple way to query some information.

The following code snippet shows a simple example how to fill such package with some properties:
``` csharp
public void FillInfoOutput(InfoOutput infoOutput, string categoryName)
{
 var package = infoOutput.AcquirePackage(categoryName);
 package.Add("a property name", 42);
}
```

 **Logger**
Again a framework for logging? Here, we design a framework only, therefore without implementation (only a dummy implementation). This allowes you to use a logging framework of your choice. For example one can use the basic logging functionality of the .net framework (`Trace`) or a specific logging framework as for example [NLog](http___nlog-project.org_) etc. One has to write a specific implementation of the `ILogger` interface.
The [Managed Extensibility Framework](http___en.wikipedia.org_wiki_Managed_Extensibility_Framework) (MEF) is used to dynamic link the external logging library to the Dodoni.net framework. Therefore one has to apply the `Export` attribute of the MEF framework to an individual implementation, i.e.

``` csharp
[Export(typeof(ILogger))](Export(typeof(ILogger)))
```

Moreover in the configuration file (`<filename.dll/exe.config>`) one has to store the name of the class that serves as wrapper for the specified logging.

``` xml
﻿<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <section name="LoggingSetting" type="Dodoni.BasicComponents.Logging.Configuration.LoggingConfigurationFileSection, Dodoni.BasicComponents"/>
  </configSections>
  <LoggingSetting typeName="YourNamespace.YourClass, YourAssemblyName" />
</configuration>
```

The static class `Logger` provides methods for adding individual log-messages. Moreover one can create a specific logging stream which is perhaps linked to a specific file etc. (which depends on the individual implementation). The following code snippet shows how to access to the _main_ logging stream (for example a global logfile vs. local logfiles for specific purpose):

``` csharp
Logger.Stream.Add(messageType, "Some text");
```

**_Design pattern for logging_**
The example above shows that a specific message type is required while adding a specified message into the logging. If one writes extension methods the application is more natural, i.e. one may write `Logger.Stream.Add_Error_CalibrationFailure("Some text");` for an error that occurred in some calibration. Extension methods are not applictable to static classes, therefore we apply it to `Logger.Stream` instead of `Logger`. The following code indicates how to implement such message types:

``` csharp
public static partial class LoggingExtensions
 {
  [Export(typeof(Logger.MessageType))](Export(typeof(Logger.MessageType)))
  public class CalibrationFailureErrorMessageType 
                       : Logger.MessageType.Error
  {
   internal static Logger.MessageType.Error Value 
                   = new CalibrationFailureErrorMessageType();

   public CalibrationFailureErrorMessageType()
    : base("Calibration failure", 
              new Guid(0x46ce269c, 0x40a6, 0x4a4a, 0xb2, 
                             0xf8, 0x44, 0xb0, 0x91, 0x89, 0x27, 0xaf))
     {
     }
 }

 public static void Add_Error_CalibrationFailure(this ILoggerStream loggingStream, 
                                   string message = null, 
                                   Exception exception = null)
 {
   loggingStream.Add(CalibrationFailureErrorMessageType.Value, 
                                message, 
                                exception);
 }

 public static void Add_Error_CalibrationFailure(this ILogger logger, 
                     string message = "", 
                     string senderObjectTypeName = "", 
                     Type senderObjectType = null, 
                     string senderObjectName = "", 
                     Exception exception = null)
 {
     logger.Add(CalibrationFailureErrorMessageType.Value, 
                            message, 
                            senderObjectTypeName, 
                            senderObjectType, 
                            senderObjectName, 
                            exception);
 }
}
```

 **ConfigurationFile**
The information which external libraries to load, the configuration of the Excel Add-In etc. of the Dodoni.net library are stored in a dedicated configuration file. You can use the same framework for your individual purpose, i.e. create a specific configuration file or share a common configuration file:

``` csharp
var configFile = ConfigurationFile.Create("fileName", "settings"); 
// 'ConfigurationFile.Dodoni' to use the same configuration file as the dodoni.net library
var propertyCollection = configFile.GetPropertyCollection("Example");
propertyCollection.SetValue("propName", "propValue");

configFile.Save();
```

**StringAttribute, LanguageStringAttribute, DescriptionAttribute, EnumString etc.**
It is well-known that the `enum` keyword is used to declare an enumeration, thus a distinct type consisting of a set of named constants called the enumerator list. In .net (in contrast to Java) it is not possible to overwrite the `ToString()` method of an enumerator. Sometimes this is a desired feature. For example an enumerator may not contain white space in its name, thus its `string` representation has similar restrictions etc.

One solution is the use of attributes and to wrap the enumeration (with class `EnumString` and `EnumString<T>`). The attributes allowes a language depending string representation taken into account a specified resource file. Moreover one can use attributes to store a short description:

``` csharp
// simple example for EnumString and extension method usage of enumerations:
enum Color 
{
   [String("light-blue")](String(_light-blue_))
   LightBlue,

   [String("red")](String(_red_))
   Red,

   [String("white")](String(_white_))
   White
}

var enumString = EnumString.Create<Color>(Color.LightBlue,EnumStringRepresentationUsage.StringAttribute);
// wraps the enumerator, the ToString() method will return "light-blue"

var str1 = Color.LightBlue.ToFormatString(EnumStringRepresentationUsage.ToStringMethod);
var str2 = Color.LightBlue.ToFormatString(EnumStringRepresentationUsage.StringAttribute);
var str3 = Color.LightBlue.ToFormatString(EnumStringRepresentationUsage.LanguageStringAttribute);
// Results: str1 = "LightBlue", str2 ="light-blue", str3 = "light-blue"
```

For language depending string representation one has to create corresponding resource files. For the following example a resource `EnumTestResources` is required, that contains for example values for the properties `Monday_Test`, `MondayDescription` etc.

``` csharp
// for the following create a resource of name 'EnumTestResources' with entries "Sunday_String", "Monday_Test" etc.
[LanguageResource("YourNamespace.EnumTestResources")](LanguageResource(_YourNamespace.EnumTestResources_))
enum Weekday
{
   [LanguageString("Sunday_String")](LanguageString(_Sunday_String_))
   Sunday,

   [String("Montag")](String(_Montag_))
   [LanguageString("Monday_Test")](LanguageString(_Monday_Test_))
   [LanguageDescription("MondayDescription")](LanguageDescription(_MondayDescription_))
   [Description("This attribute should be ignored, 'LanguageDescription' will be prefered.")](Description(_This-attribute-should-be-ignored,-'LanguageDescription'-will-be-prefered._))
   Monday,

   [LanguageString("Tuesday")](LanguageString(_Tuesday_))
   [Description("A simple description....")](Description(_A-simple-description...._))
   Tuesday
 }

var str4 = Weekday.Sunday.ToFormatString(EnumStringRepresentationUsage.ToStringMethod);
var str5 = Weekday.Sunday.ToFormatString(EnumStringRepresentationUsage.StringAttribute);
var str6 = Weekday.Sunday.ToFormatString(EnumStringRepresentationUsage.LanguageStringAttribute);
// Results: str4 = "Sunday", str5 = "Sunday", str6 = <depends on the value in the resource file>

var str7 = Weekday.Monday.ToFormatString(EnumStringRepresentationUsage.ToStringMethod);
var str8 = Weekday.Monday.ToFormatString(EnumStringRepresentationUsage.StringAttribute);
var str9 = Weekday.Monday.ToFormatString(EnumStringRepresentationUsage.LanguageStringAttribute);
// Results: str7 = "Monday", str8 = "Montag", str9 = <depends on the value in the resource file>

var str10 = Weekday.Tuesday.GetDescription();
var str11 = Weekday.Monday.GetDescription();
// Results: str10 = "A simple description....", str11 = <depends on the value in the resource file>
```
