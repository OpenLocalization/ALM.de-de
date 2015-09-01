Toc: show
Parent: ./index.md
Title: Build your Visual Studio solution
ShortTitle: Visual Studio solution
ms.TocTitle: Visual Studio solution
ms.ContentId: 2BFC43A2-4F6C-4A5C-86EE-6DDA8733829D

#Build your Visual Studio solution

After you have [deployed a Windows build agent](/Library/vs/alm/Build/agents/windows.md), you are ready to define a CI build that compiles and tests your Visual Studio solutions whenever your team checks in code.

<iframe width="420" height="315" src="https://www.youtube.com/embed/Jx8s7KAATH4" frameborder="0" allowfullscreen="true" caps_internal_Id="c1fa57a6-c5f9-4fe6-b7e5-e060f6a76978" />

##Create the definition

1.  Create the build definition.
    
    ![Build tab](../_shared/_img/web-portal-build-tab.png)
    
    ![New Visual Studio build](_img/new-visual-studio-build-from-definition-templates.png)
2.  Select the continuous integration (CI) trigger and specify the code you want to build
    
    ![CI trigger](../_shared/_img/build-trigger-ci-master-batch.png)
3.  Save the definition.
    
    ![Save button](../_shared/_img/build-definition-save-button.png)
    
    ![Save the build](../_shared/_img/BldSave.png)
4.  Queue your new definition to make sure it works.
    
    ![Queue the build](../_shared/_img/queue-build-dialog-box-with-hosted.png)
    
    ![Completed build](_img/visual-studio-build-completed.png)

[!INCLUDE [temp](../_shared/definition-draft.md)]

[!INCLUDE [temp](../_shared/definition-replicate.md)]

##Q&A


####How do I customize my build?

[Run a PowerShell script](/Library/vs/alm/Build/scripts/index.md)

####How do I combine check-ins to run fewer builds?

On the Triggers tab, select Batch changes.
When you select this option, the build system runs only one build per branch at a time.
Any pushes or check-ins that occur while a build is running are combined and built together when the current build is completed.

####How do I automatically purge old builds?

If you are using Visual Studio Online, you can specify retention rules.
This feature is not yet available for on-premises Team Foundation servers.

![Retention rules](/library/vs/alm/build/_shared/_img/BldRetRuleExample.png)

####How do I build multiple configurations for multiple platforms? Can I build them in parallel?

Select MultiConfiguration to build multiple configurations for multiple platforms.
Select Parallel if you want to distribute the jobs (one for each combination of values) to multiple agents in parallel if they are available.

![MultiConfiguration](_img/BldOptionMultiConfig.png)

Make sure to declare and assign values to the variables.
To specify multiple values, separate them with commas.

![config and platform build variables](_img/BldVSBuildVarsPlatformConfig.png)

Pass the variables to to the Visual Studio build step so that it passes them as arguments to MSBuild.

![Visual Studio build platform and configuration paramaters](_img/BldVSBuildStepPlatformConfig.png)

[!INCLUDE [temp](/library/vs/alm/build/_shared/qa-variable-secret.md)]



