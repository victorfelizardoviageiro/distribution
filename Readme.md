# Amzi Prolog Distribution
This repository contains the binaries needed to integrate Amzi Prolog and Logic Server into your applications.

## .NET
There is a [NuGet package](https://www.nuget.org/packages/amzinet/0.1.0) that can easily be pulled into your project. The .NET binary package consists of two DLLs, one is the .NET wrapper called amzinet.dll and other is amzi.dll which is the Logic Server. amzi.dll should be deployed to your bin directory. The NuGet package is set up to make your project do just that. Besides NuGet, .NET binaries could also be found in one of the Windows archives found in this repository.

### ASP.NET Web Applications
There is an extra step required to make ASP.NET web application run smoothly with Amzi Prolog and Logic Server. ASP.NET uses what is known as [Shadow Copy](https://en.wikipedia.org/wiki/Shadow_Copy), which copies all the binaries necessary for the project into a preliminary directory where they are run from (typically, it is `Temporary ASP.NET Files`). The Shadow Copy process copies only the CLR DLLs and their references, it will not copy the amzi.dll. For that to be picked up by the application, the only alternative is to deploy the amzi.dll into one of the directories included in the `PATH` environment variable. A better way if you do not have access to any of those directories -- such as when you are deploying your application to Microsoft Azure Cloud services -- is that you add your application pool's bin directory to the `PATH` environment variable at the `Application_Start` event.

```csharp
string path = Environment.GetEnvironmentVariable("PATH");
string binDir = Path.Combine(AppDomain.CurrentDomain.BaseDirectory, "Bin");
if (!path.EndsWith(";"))
{
    path = path + ";";
}
path = path + binDir;
Environment.SetEnvironmentVariable("PATH", path);
```