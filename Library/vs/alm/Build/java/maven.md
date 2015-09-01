Toc: show
Parent: ./index.md
Title: Build your Java project with Maven
ShortTitle: Java with Maven
ms.TocTitle: Java with Maven
ms.ContentId: C339FAF9-A960-4A3A-9A8A-ADCD39C2703D

#Build your Java project with Maven

After you have deployed a [Windows build agent](../agents/windows.md) or an [Xplat build agent](../agents/xplat.md), you are ready to define a CI build that compiles and tests your Java project with Maven whenever your team checks in code.

##Create the definition

1.  Create the build definition.
    
    ![Build tab](../_shared/_img/web-portal-build-tab.png)
    
    ![New Java build](_img/new-java-build-from-definition-templates.png)
2.  Add the Maven build step.
    
    ![Add build step](_img/add-build-step.png)
    
    ![Add Maven build step](_img/add-build-step-maven.png)
3.  Provide the path to your Maven POM file.
    
    ![Maven build step](_img/maven-build-step.png)
4.  Select the continuous integration (CI) trigger and specify the code you want to build.
    
    ![CI trigger](../_shared/_img/build-trigger-ci-master-batch.png)
5.  Save the definition.
    
    ![Save button](../_shared/_img/build-definition-save-button.png)
    
    ![Save the build](../_shared/_img/BldSave.png)
6.  Queue your new definition to make sure it works.
    
    ![Queue the build](_img/queue-build-dialog-box-with-hosted.png)
    
    ![Completed build](_img/eclipse-build-completed.png)

Since this is a CI build, a build is started every time you push a commit to one of the branches specified on the Triggers tab.

##Q&A

[!INCLUDE [temp](../_shared/qa-variable-secret.md)]



