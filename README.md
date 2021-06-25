# MSTestX

A cross-platform .NET Standard compilation of the MS Test Adapter, and a Xamarin Test Runner.

This isn't "just another test framework". This is all based on the Microsoft MSTest Framework, so that your unit tests will run and behave the exact same way as your .NETFramework and UWP Unit Tests. I was fed up with having to deal with different test frameworks all doing things slightly different, and spending too much time abstracting those differences away, and dealing with different report formats.

### Features

- Uses the same asserts and test attributes from [MSTest.Framework NuGet package](https://www.nuget.org/packages/MSTest.TestFramework/)
- Supports automation from commandline
- Supports generating a TRX Report identical to those `VSTest.Console.exe` generates for easier integration into existing reporting systems.

## Usage

1. Inside your solution, create a new blank Xamarin.Forms Project targeting iOS and Android (shared or .NET Standard)
2. In NewProject.Android: 
   A) Delete `MainPage.xaml` and `App.xaml`
   B) In MainActivity.cs file change the class to inherit from `MSTestX.TestRunnerActivity`
   C) In MainActivity.cs file, remove all code from OnCreate except base.OnCreate line
3. In NewProject.iOS: 
   (A) Change the AppDelegate to inherit from `MSTestX.TestRunnerApplicationDelegate`
4. In the new common project (blank projects parent aka NewProject, not NewProject.Droid or NewProject.iOS):
   A) Add a Nuget reference to [`MSTestX.UnitTestRunner`](https://www.nuget.org/packages/MSTestX.UnitTestRunner)
   B) Add a unit test class with the following content:

```cs
using Microsoft.VisualStudio.TestTools.UnitTesting;

namespace MyUnitTestApp
{
    [TestClass]
    public class Tests
    {
        [TestMethod]
        [TestCategory("Simple Tests")]
        public void MyTest()
        {
            Assert.IsTrue(true);
        }
    }
}
```

Note: Tests in other projects in the same solution will be found as well

Note: if you put your tests in a class library, the iOS app, will need to reference one of the types in the AppDelegate, or the compiler will strip out the unit test DLL (this isn't an issue if you use a shared project with tests).

Note: This is not a fork. The submodule literally uses the code as-is from TestFX but compiled so it can run and be referenced by a Xamarin App.

## How To Run

To launch, right click on new MSTest project NewProject.Android and click 'Debug > Start new instance'

### Automation

On Android you can build, deploy, run and generate a TRX report with the the console runner:

```
msbuild myproject.csproj
dotnet tool install --global MSTestX.Console --version 0.16.2
REM Deploy the app and run all tests
mstestx.console -apkpath path-to-app-signed.apk
REM Connect to an already running app on IP 192.168.1.200 (also works with iOS)
mstestx.console -remoteIp 192.168.1.200:38300
REM Connect and launch  an already installed app
mstestx.console -apkid [package id] -activity [activity name]
```

The NuGet package also contains a console app in the `tools\` folder useful for automating the unit test run. Android has the most capability including deploy, launch and monitoring. For both iOS and Android you can connect to an already running Unit Test app using the `/remoteIp deviceip:38300` command-line parameter.

### Screenshots

![image](https://user-images.githubusercontent.com/1378165/43662635-757007ee-971b-11e8-9b10-63c1d2983385.png)

![image](https://user-images.githubusercontent.com/1378165/43662619-65fa0a4e-971b-11e8-9059-51c86522103d.png)

![image](https://user-images.githubusercontent.com/1378165/43662682-9514fbb8-971b-11e8-9c67-a46ff7290e0d.png)
