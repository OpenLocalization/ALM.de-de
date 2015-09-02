Title: Publish build artifacts
Description: Publish build artifacts
ms.TocTitle: Publish build artifacts
ms.ContentId: 1BB6BB93-5749-438F-B0BD-B8DB61141D9A

#Publish build artifacts

Copy binaries and other files you need from your build process into an artifact on your server or file share so your team can download them.

##Demands

None

##Arguments

<table>
  <thead>
    <tr>
      <td caps_internal_Id="9991334e-d0d2-4c0a-b149-db3ff94502c1">Argument</td>
      <td caps_internal_Id="edc0297a-0176-45fb-bba6-5cabf379fc61">Description</td>
    </tr>
  </thead>
  <tr>
    <td caps_internal_Id="b6c5f316-68d3-4147-b3a6-e4d39897d447">Copy Root</td>
    <td>
      <p caps_internal_Id="3991522b-8e72-4347-89a1-ee1a5e243f97">Specify the root folder that contains the files you want to copy.</p>
      <p caps_internal_Id="ec48c040-6646-44bd-bc07-eb60485d39b4">Leave it empty to copy from the root folder of the repo. For example ```C:\a\```&lt;randomid&gt;```\OurRepo```</p>
      <p caps_internal_Id="b24d26ff-d589-465f-9408-4a203ffd6d87">Specify $(Agent.BuildDirectory) if your build produces artifacts outside of the sources directory. This will copy files from the build agent working directory. For example ```C:\a\```&lt;randomid&gt;</p>
    </td>
  </tr>
  <tr>
    <td caps_internal_Id="4aee1b2a-4942-4189-a6be-c683a07c93eb">Contents</td>
    <td>
      <p caps_internal_Id="52cbb8b3-32bb-4562-a313-af6f54a30a7e">Specify minimatch pattern filters (one on each line) that you want to apply to the list of files to be copied. For example, specify ```**\bin``` to copy files in any sub-folder named bin.</p>
    </td>
  </tr>
  <tr>
    <td caps_internal_Id="267717e3-152f-4869-9e7a-f42838f57766">Artifact Name</td>
    <td caps_internal_Id="782be66b-d393-4f06-a9ab-11fdafb96064">Specify the name of the artifact.</td>
  </tr>
  <tr>
    <td caps_internal_Id="81eed42c-c9b5-4dc7-8b47-5eaed919a823">Artifact Type</td>
    <td>
      <p caps_internal_Id="1dd83e9f-9184-430f-9fdb-f7fa2cd58879">Choose server to store the artifact on your Team Foundation Server. This is the best and simplest option in most cases.</p>
      <p caps_internal_Id="7952e37b-397b-47d7-86fe-01adc9970b15">Choose file share to copy the artifact to a file share. Some common reasons to do this:</p>
      <ul>
        <li caps_internal_Id="b7690485-43f2-4829-8cd2-e51455dfb136">The size of your drop is large and consumes too much time and bandwidth to copy.</li>
        <li caps_internal_Id="9e9bfa41-1ab7-4f0e-aef5-98feac3cc21e">You need to run some custom scripts or other tools against the artifact.</li>
      </ul>
      <p caps_internal_Id="5c7dc5fb-b552-4883-b0b7-e6efe9f33906">
If you use a file share, specify the UNC file path to the folder. You can control how the folder is created for each build using variables. For example ```\\my\share\$(Build.DefinitionName)\$(Build.BuildNumber)```. 
</p>
      <p caps_internal_Id="e7630b9b-af4d-4d5a-97d6-cbee9ef6b31f">**Note:** You cannot use a file share if you are using the [hosted pool](https://www.visualstudio.com/get-started/build/hosted-agent-pool).</p>
    </td>
  </tr>
</table>

##Examples

###Copy executables and readme file

####Goal

You want to copy just the read me and the files needed to run this C# console app:

*   Solution 'ConsoleApplication1' (3 projects)
    
    *   C# ClassLibrary1
    *   C# ClassLibrary2
    *   C# ConsoleApplication1   

####Arguments

*   Copy Root: $(Build.SourcesDirectory)
*   Contents example 1: 


   ```
   ConsoleApplication1\bin\**\*.exe
   ConsoleApplication1\bin\**\*.dll
   readme.txt

   ```


*   Content example 2:


   ```
   ConsoleApplication1\bin\**\?(*.exe|*.dll)
   readme.txt

   ```


*   Artifact Name: drop

####Results

The *drop* artifact is published and contains these files:

*   ConsoleApplication1
    
    *   bin
    *   Release
        
        *   ClassLibrary1.dll
        *   ClassLibrary2.dll
        *   ConsoleApplication1.exe
*   readme.txt

##Q & A


###Q: I'm having problems. How can I troubleshoot them?

A: Try this:

1.  On the variables tab, add `system.debug` and set it to `true`.
    Select to allow at queue time.
2.  In the explorer tab, view your completed build and click the publish artifact step to view the output.

###Q: What's a minimatch pattern? How does it work?

See https://github.com/isaacs/minimatch and https://realguess.net/tags/minimatch/.

###Q: What variables can I use?

A: [Predefined variables](../../scripts/variables.md)



