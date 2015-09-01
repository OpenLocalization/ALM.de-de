Title: Overview of Team Foundation Build 2015
ms.TocTitle: Overview
toc: show
Parent: ./index.md
ms.ContentId: 2F76AECB-D1A4-4E29-9956-7C7A75D89B74

#Overview of Team Foundation Build 2015 (Beta)

No matter what tools you use.
In whatever language you prefer.
Team Foundation Build (TFBuild) 2015 builds your app, your way, for your platforms.
Just open your web browser to tell us how you want it done.

##Build for your platform, speaking your language

Build for Windows, iOS, Android, Java (Ant, Maven, or Gradle), or Linux using the same domain-specific languages (DSL) you use every day on your dev machine.
We even build Xamarin apps for both iOS and Android and run tests on Xamarin's Test Cloud as part of the build.
We intend to enable publishing the Test Cloud test results to the server so you can view them in your build summary.

![New Image](_img/cdn-1-browser-access.PNG)

![Xplat](_img/overview/add-build-steps.png)

![BldVNext](_img/5star2.png)

##Easy customization

Edit in the web and leverage existing knowledge of popular script 
languages.
No need to ramp up on XAML workflow to customize the build.

![Add script tasks](_img/overview/add-build-steps-utility.png)

[//]: # "Add shot of a step - PowerShell as shown in blog seems good, yes? - and note that you just have to set a few properties. And with VS template, you don't have to set any props for simplest cases"

##Extensible cross-platform tasks and build engine

This web- and script-based build system works with both your [windows agents](agents/windows.md) and [Xplat agents (runs on Mac and Linux systems)](agents/xplat.md) .
The Xplat agent is [an open-source Node.js app](https://github.com/Microsoft/vso-agent).

If you need to add a task we don't currently offer, you can create
your own activity using our [open source activities](https://github.com/Microsoft/vso-agent-tasks) 
as examples of best practices.

##Run unit tests and publish results

Run unit tests using just about any framework you like including NUnit, MSTest, and JUnit.
Publish test results to and include them in the build summary.

![Publish test results](_img/overview/publish-test-results.png)

##Real-time build status

Live console view in the web with real time status of each task.

![History](_img/overview/live-console-view-during-build.png)

We also provide real-time status and time breakdowns project by project

![Timeline](_img/overview/timeline-view-during-build.png)

##Definition auditing and diff

Know who changed what in the build definition and when they did it.

![Build definition history](_img/overview/build-definition-history.png)

![Diff in build definition history](_img/overview/build-definition-history-diff.png)

##Elastic use of self-updating resources

Agents can span team project collections.
Side-by-side installs and remote agents are xcopy (download from the web UI).
No need to install TFS on your build machines.

You can stop running around and worrying about updating your build agents when we release a new version.
Visual Studio Online (or your on-premises Team Foundation Server) automatically distributes agent updates, leaving in place whatever capabilities (such as compilers, SDKs, or tools) you've installed on those machines.

##Better control of agent routing

Definitions route to the right agent with capabilities needed by the definition.

![Agent capabilities](_img/overview/agent-capabilities.png)

##Secret variables

Lock a variable to hide it from the web and store it in a strong box.

![Secret](/Library/vs/alm/Build/_shared/_img/BldVarSecret.png)

##Azure continuous deployment improved

Publish multiple web sites and cloud services in a single build.
You can now deploy from your on-premises Team Foundation Server to Azure (XAML builds deploy only from Visual Studio Online).

![Azure build definition templates](_img/overview/azure-deployment-templates.png)

##CI improved

Monitor and track multiple branches.
Associate your builds with each branch.

!(_img/overview/ci-build-trigger-with-multiple-branches.png)

(And you can use your CI build as your nightly scheduled build too.)

##Build matrix

Multiply build jobs by any set of variables.
Optionally run in parallel across multiple agents.

![Matrix](_img/matrix.png)

##True templates

Most build scenarios are new definition, couple inputs, go.
Save as custom templates.

![Template](_img/template.png)

##Definition drafts

To test some changes that are not yet ready for production, save a draft.

![Save as a draft](/Library/vs/alm/Build/_shared/_img/BldDefSaveDraft1.png)

When you're ready you can publish and keep the changes or delete them.

##Clone definitions

Definitions can be cloned.

![Draft](_img/draft.png)

##Q&A

[!INCLUDE [temp](_shared/qa-use-in-production.md)]

[!INCLUDE [temp](_shared/qa-new-old-choose.md)]

[!INCLUDE [temp](_shared/qa-new-old-relate.md)]

####Can I use this system to chain builds so that one process is triggered by another?

Not yet.


