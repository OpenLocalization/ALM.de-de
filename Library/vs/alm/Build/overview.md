Title: Team Foundation Build
ms.TocTitle: Build
Description: Build Windows, Android, xCode and many other kinds of apps on Team Foundation Server and Visual Studio Online.
ms.ContentId: B1233296-C583-4F2E-981C-82D6A39CFEE4
ms.topic: code reference (API)
Toc: show

#Test Team Foundation Build

![Customizable.
Cross Platform.](_img/taskbanner.png)<br/>

<div style="background-color:#e4c1ee; padding:7px; margin:10px">
We've built a brand new scriptable build system that's web-based and cross-platform. We believe all new and most existing customers should use it instead of the XAML build system. <b caps_internal_Id="45a23b9b-c404-41d2-aabd-1e9cc951b671">[Tell me more.](feature-overview.md)</b></div>

##Build agents

You need at least one agent to build your code.

*   [Deploy Windows agents](agents/windows.md) to build Windows, Azure, and other Visual Studio solutions
*   [Deploy Xplat agents](agents/xplat.md) to build Xcode, Android, Java, or other kinds of apps
*   [Scale out and administer your build system](agents/admin.md)

##Build apps for Windows or Azure

*   [Build your Windows solution](vs/define-build.md)
*   [Build and deploy to Azure](azure/index.md)
*   [Build and Deploy your ASP.NET 5 Application to an Azure Web App](azure/deploy-aspnet5.md)

##Build Java projects

*   [Build your Java project with Maven](java/maven.md)

##Build Xcode apps for iOS

*   [Build your Xcode project](xcode/index.md)

##Build your GitHub-hosted projects in Visual Studio Online

*   [Build your GitHub-hosted projects in Visual Studio Online](github/index.md)

##Customize your build process using a script

*   [Run a script](scripts/index.md)
*   [Predefined variables you can use in your scripts](scripts/variables.md)

##Build steps

*   [Publish build artifacts](steps/build/publish-build-artifacts.md)

##Q&A

[!INCLUDE [temp](_shared/qa-use-in-production.md)]

[!INCLUDE [temp](_shared/qa-new-old-choose.md)]

See [Overview](feature-overview.md).

[!INCLUDE [temp](_shared/qa-new-old-relate.md)]

####Can I extend the build tasks? Are they open source?

Yes: [Microsoft vso-agent-tasks](https://github.com/Microsoft/vso-agent-tasks).



