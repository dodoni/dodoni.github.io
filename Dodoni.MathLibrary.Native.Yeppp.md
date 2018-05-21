## `Dodoni.MathLibrary.Native.Yeppp`

#### 1. Overview
Provides wrapper for [Yeppp!](http://www.yeppp.info/),  a high-performance SIMD-optimized mathematical library for x86, ARM, and MIPS processors on Windows, Android, Mac OS X, and GNU/Linux systems.

#### 2. Dependencies
This assembly depends on 
* [Dodoni.BasicComponents](BasicComponents)
* [Dodoni.BasicMathLibrary](BasicMathLibrary)

#### 3. Main concepts and helpful code snippets
The [Managed Extensibility Framework](http://en.wikipedia.org/wiki/Managed_Extensibility_Framework) (MEF) is used to dynamic link to this assembly. If specific features are needed which are not covered by the generic interface one may add a reference to [yeppp-cli.dll](http://docs.yeppp.info/cs/index.html#GettingStarted) which is part of the distribution of Yeppp! One may have a look into the unit test project of {{Dodoni.MathLibrary.Native.Yeppp}}; see API documentation for more information.


#### 4. Important remarks
* Yeppp! does not support all Vector operations which are provided by the Dodoni framework, especial operations which complex numbers are limited. In this cases a (slow) managed implementation is taken as a fall back solution. 
* In some cases release 1.0 of Yeppp! throws some {{MisalignedPointerException}}. Therefore it is recommended to use release 1.0.1 (preview release), which is available in [NuGet](https://www.nuget.org/). The CLR DLL contains the native libraries as embeded resources which must be extracted for the use in the Dodoni framework. Thanks to private communication with Marat Dukhan, the lead developer of Yeppp!, one can use the following code snippets for the extraction.; see [Loader.cs](https://bitbucket.org/MDukhan/yeppp/src/b8db687e912bbd7e2a26dd93c348ef2d7a5febdc/bindings/clr/sources-csharp/library/Loader.cs?at=default#cl-183). In the future the native library will again be part of the distribution, therefore this workaround should be applied to Yeppp! 1.0.1 only.

{code:c#}
public static void ExtractResource(string outputPath)
{
  var resource = "windows/x86/yeppp.dll";  // "windows/x86_64/yeppp.dll";
   try
   {
    var assembly = Assembly.LoadFrom(@"[Path..](Path..)/Yeppp.CLR.Bundle.dll");

     using (Stream resourceStream = assembly.GetManifestResourceStream(resource))
      {
       using (DeflateStream deflateStream = new DeflateStream(resourceStream, CompressionMode.Decompress))
        {
          using (FileStream fileStream = new FileStream(outputPath, FileMode.CreateNew, FileAccess.Write, FileShare.None))
           {
             byte[]() buffer = new byte[1048576](1048576);
             int bytesRead;
             do
              {
               bytesRead = deflateStream.Read(buffer, 0, buffer.Length);
               if (bytesRead != 0)
                     fileStream.Write(buffer, 0, bytesRead);
               } while (bytesRead != 0);
             }
          }
       }
     }
     catch
     {
      File.Delete(outputPath);
      }
 }
{code:c#}

