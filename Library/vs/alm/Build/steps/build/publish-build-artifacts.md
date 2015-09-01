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
      <td caps_internal_Id="d0c7fa9e-0ef3-4aab-ba58-50f5e5824ae1">Argument</td>
      <td caps_internal_Id="473dfc12-bef9-4364-988a-0c9854d16e68">Description</td>
    </tr>
  </thead>
  <tr>
    <td caps_internal_Id="2af7a6e4-1c4b-4c86-9723-c8d5e2ee574b">Copy Root</td>
    <td>
      <p caps_internal_Id="b4d2431a-7451-445d-acfe-25220262223c">Specify the root folder that contains the files you want to copy.</p>
      <p caps_internal_Id="9e3e1954-fe3f-4d90-b0ac-efac276e4825">Leave it empty to copy from the root folder of the repo. For example ```C:\a\```&lt;randomid&gt;```\OurRepo```</p>
      <p caps_internal_Id="7a7ba5eb-2839-4da8-adb5-9edea8954821">Specify $(Agent.BuildDirectory) if your build produces artifacts outside of the sources directory. This will copy files from the build agent working directory. For example ```C:\a\```&lt;randomid&gt;</p>
    </td>
  </tr>
  <tr>
    <td caps_internal_Id="c1f7d34d-0e68-45d5-a09e-61d919cbd0f8">Contents</td>
    <td>
      <p caps_internal_Id="eab3df9b-031b-4f17-a655-18f8d075597f">Specify minimatch pattern filters (one on each line) that you want to apply to the list of files to be copied. For example, specify ```**\bin``` to copy files in any sub-folder named bin.</p>
    </td>
  </tr>
  <tr>
    <td caps_internal_Id="4abd7d5b-c9b6-43a5-8d36-e20a1b78cca4">Artifact Name</td>
    <td caps_internal_Id="2652d096-86a3-4a95-819c-07875a248faa">Specify the name of the artifact.</td>
  </tr>
  <tr>
    <td caps_internal_Id="ba767aa6-8de0-410b-aaf4-d023848bf130">Artifact Type</td>
    <td>
      <p caps_internal_Id="d7043146-c727-4bc0-b861-a23817f8081b">Choose server to store the artifact on your Team Foundation Server. This is the best and simplest option in most cases.</p>
      <p caps_internal_Id="0275a2d7-b7f7-40e7-9063-4880f31e5e5c">Choose file share to copy the artifact to a file share. Some common reasons to do this:</p>
      <ul>
        <li caps_internal_Id="b5a7cab1-51cf-4a8f-8b40-e5bd2891536e">The size of your drop is large and consumes too much time and bandwidth to copy.</li>
        <li caps_internal_Id="67c6683b-3387-4057-be7b-be0e327b88a0">You need to run some custom scripts or other tools against the artifact.</li>
      </ul>
      <p caps_internal_Id="385db2be-7071-4957-a569-c0b948a0f860">
If you use a file share, specify the UNC file path to the folder. You can control how the folder is created for each build using variables. For example ```\\my\share\$(Build.DefinitionName)\$(Build.BuildNumber)```. 
</p>
      <p caps_internal_Id="0436b608-1afc-4965-b911-8e03f53084c4">**Note:** You cannot use a file share if you are using the [hosted pool](https://www.visualstudio.com/get-started/build/hosted-agent-pool).</p>
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

A: [Predefined variables](/library/vs/alm/build/scripts/variables.md)



