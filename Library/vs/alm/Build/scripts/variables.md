toc: show
Parent: ./index.md
Title: Predefined variables
ms.ContentId: 3A1C529F-DF6B-470A-9047-2758644C3D95

#Predefined variables

Team Build provides a number predefined variables that can be used in configuring your build definition or in your build scripts.

There are a number of scopes where the variables are defined:

*   System Variables - These are generated by the server and the agent and are the same name and concept but potentially different values depending on the host type for the agent.
    For example in Build the `System.DefaultWorkingDirectory` is the same as `Build.SourcesDirectory`.
    In release management it will be the `Release.ArtifactsDirectory`.
*   Global Build Variables - These are generated by TFBuild on the server side and are sent to the agent as part of the build job.
    An example this variable would be `Build.BuildNumber`
    
    *   In addition tasks or build scripts can contribute to this collection of variables and they will be available both in the context and in the environment for downstream tasks.
*   Agent Build Variables - These are generated on the agent in the scope of a particular build job prior to it starting.
    An example of this is `Build.SourcesDirectory`.

##System Variables

<table>
  <thead>
    <tr>
      <th caps_internal_Id="79b53649-4651-42e0-98e5-b2b08e4d1d51">Variable Name</th>
      <th caps_internal_Id="7d027de7-fd3d-4006-9b10-bc1efc2e486d">Environment Variable Name</th>
      <th caps_internal_Id="d7454321-5516-49c5-a56c-e1fbe24b4f93">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p caps_internal_Id="4bbadd64-e507-4f63-a94d-adb714b40d4e">
                    System.TeamProject
                </p>
      </td>
      <td>
        <p caps_internal_Id="2607aacb-7fa8-46e5-af12-460c7432e9d1">
                    SYSTEM_TEAMPROJECT
                </p>
      </td>
      <td>
        <p caps_internal_Id="c2a7a0df-847e-45df-b6dc-ea390483228c">
                    The name of the team project that this build belongs to
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="ae80c949-1b66-448b-b34d-adb96c4410d2">
                    System.TeamFoundationServerUri
                </p>
      </td>
      <td>
        <p caps_internal_Id="9b371ead-544a-4fef-aba1-77cc020ea90f">
                    SYSTEM_TEAMFOUNDATIONSERVERURI
                </p>
      </td>
      <td>
        <p caps_internal_Id="4813db5c-43d7-4368-afc8-6a7cbef457bb">
                    The URL of the team foundation server
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="e5dd01e9-8c4c-4708-ac3f-f432fd107c67">
                    System.TeamFoundationCollectionUri
                </p>
      </td>
      <td>
        <p caps_internal_Id="0f7cb0e2-fbc8-4bf3-bf2d-dd082ff22c50">
                    SYSTEM_TEAMFOUNDATIONCOLLECTIONURI
                </p>
      </td>
      <td>
        <p caps_internal_Id="5efadd3a-6280-4ee5-b2da-a5662b7bec42">
                    The URL of the team foundation collection
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="cb43067c-d3e0-4780-b212-6da78b0be5f7">
                    System.CollectionId
                </p>
      </td>
      <td>
        <p caps_internal_Id="e0529517-5e61-4d69-94ad-9592feda23ea">
                    SYSTEM_COLLECTIONID
                </p>
      </td>
      <td>
        <p caps_internal_Id="8d37e779-b13d-4278-9a4c-bd4b7293ca27">
                    The guid id of the team foundation collection
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="f5813382-6e9f-4380-bde4-d2f41234951e">
                    System.DefaultWorkingDirectory
                </p>
      </td>
      <td>
        <p caps_internal_Id="1e8370df-8ade-46db-8343-2525a7d7cb70">
                    SYSTEM_DEFAULTWORKINGDIRECTORY
                </p>
      </td>
      <td>
        <p caps_internal_Id="cf153361-dd8d-4f72-b03d-d10b48ec441d">
                    The default working directory for this job.  In Build this is `Build.SourcesDirectory`. In Release this is `Release.ArtifactsDirectory`
                </p>
      </td>
    </tr>
  </tbody>
</table>

##Global Build Variables

<table>
  <thead>
    <tr>
      <th caps_internal_Id="8e836141-9ec7-4b61-b0c2-6ad017b1c612">Variable Name</th>
      <th caps_internal_Id="0d154cdd-9f25-41cb-93e8-2f86cb1467b8">Environment Variable Name</th>
      <th caps_internal_Id="7981d110-1533-414b-9b51-c3a696bb19d4">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p caps_internal_Id="fadec493-0d63-4a2a-921b-6c963ac32297">
                    Build.DefinitionName
                </p>
      </td>
      <td>
        <p caps_internal_Id="17a888e2-0610-4476-bbdf-4a1927373ada">
                    BUILD_DEFINITIONNAME
                </p>
      </td>
      <td>
        <p caps_internal_Id="dba5e499-a5e4-4d68-8fab-a74c95cc67ec">
                    The name of the Build Definition the current build belongs to
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="427ab00e-9ea3-42d7-a579-729fe9e429b1">
                    Build.DefinitionVersion
                </p>
      </td>
      <td>
        <p caps_internal_Id="755179f1-9720-4cfb-ad65-a501802433b8">
                    BUILD_DEFINITIONVERSION
                </p>
      </td>
      <td>
        <p caps_internal_Id="e36809c0-48a8-4d8c-bb88-0821a68c77f1">
                    The version of the Build Definition the current build belongs to
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="889907ee-37eb-4927-a705-9dbfb8e62a88">
                    Build.BuildNumber
                </p>
      </td>
      <td>
        <p caps_internal_Id="24bc2ef9-690b-4b30-9e2d-84dae008cb36">
                    BUILD_BUILDNUMBER
                </p>
      </td>
      <td>
        <p caps_internal_Id="6d32e262-9543-4f63-be2e-f7fb868c0be8">
                    The build number assigned to the build by TFS using the build number format specified.
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="037321ed-5b47-4436-a21c-59f4a401a7e1">
                    Build.BuildUri
                </p>
      </td>
      <td>
        <p caps_internal_Id="65293373-bdeb-4c65-b882-c22d7b551396">
                    BUILD_BUILDURI
                </p>
      </td>
      <td>
        <p caps_internal_Id="9ac554cb-330d-4737-90cb-f917b9867ff9">
                    The Uri for the build
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="0bff4123-14ec-4b03-9eca-21664372302a">
                    Build.BuildId
                </p>
      </td>
      <td>
        <p caps_internal_Id="9706f1a7-5a57-4ff3-8b7c-50a8ecd7473c">
                    BUILD_BUILDID
                </p>
      </td>
      <td>
        <p caps_internal_Id="dd325272-a4f2-4ea8-9b36-eb8f26471041">
                    The Id of the build record
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="bde51bfe-f4df-418f-b5f3-172d9766205a">
                    Build.QueuedBy
                </p>
      </td>
      <td>
        <p caps_internal_Id="0ffa8a73-7741-482f-9c67-f07cafae9e00">
                    BUILD_QUEUEDBY
                </p>
      </td>
      <td>
        <p caps_internal_Id="78423f15-d61a-46dc-afd1-e3df14d14443">
                    The identity of the user who queued the build.
                </p>
        <ul>
          <li caps_internal_Id="0d430cea-e2e0-4105-ab87-106b93c097b6">In the case of CI and Scheduled this will simply be the system identity</li>
          <li caps_internal_Id="3bdb313a-22c1-4e3b-bc87-98022bf86f1d">For manual this will be the identity of the user who queued the build</li>
          <li caps_internal_Id="ab0ba607-4274-4008-84b8-76e1b5c858fc">For external triggers it will be the identity used to authenticate to the service</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="858e9d95-d4c2-4e22-84e2-2c007d01be93">
                    Build.QueuedById
                </p>
      </td>
      <td>
        <p caps_internal_Id="060678ae-3768-4293-b44c-c5a68cc3e156">
                    BUILD_QUEUEDBYID
                </p>
      </td>
      <td>
        <p caps_internal_Id="5dfcd8b3-0df8-4fe0-ad77-c34b10d6192b">
                    The giud id of the user who queued the build.
                </p>
        <ul>
          <li caps_internal_Id="d0135f79-e3ef-40ae-bcb4-19d5ae04579a">In the case of CI and Scheduled this will simply be the system identity</li>
          <li caps_internal_Id="be3dae3c-7f5e-49eb-90a8-e3d05cc0015f">For manual this will be the identity of the user who queued the build</li>
          <li caps_internal_Id="ac20bc25-4f46-427a-8757-3df9d27269ee">For external triggers it will be the identity used to authenticate to the service</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="239589a5-f411-41b4-b66e-057647786319">
                    Build.RequestedFor
                </p>
      </td>
      <td>
        <p caps_internal_Id="63727827-9533-4c8a-a5ed-13433c3795a3">
                    BUILD_REQUESTEDFOR
                </p>
      </td>
      <td>
        <p caps_internal_Id="1419f28a-845f-4141-8dd9-102a0e771735">
                    This user the build was requested for. In a CI build this will be the user who performed the check-in that triggered the build.
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="bc8ff221-1943-4e90-882d-ac4e3d06ca99">
                    Build.RequestedForId
                </p>
      </td>
      <td>
        <p caps_internal_Id="87db99f5-1915-48b3-ac35-a47651f57c1d">
                    BUILD_REQUESTEDFORID
                </p>
      </td>
      <td>
        <p caps_internal_Id="1e6d04a2-d7c9-4b0d-9f81-21356b0c21ce">
                    This guid id of the user the build was requested for. In a CI build this will be the user who performed the check-in that triggered the build.
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="0161f1c4-f871-46f5-98bd-2807b877252e">
                    Build.SourceVersion
                </p>
      </td>
      <td>
        <p caps_internal_Id="d16faa74-9390-46e4-b961-f5f010940382">
                    BUILD_SOURCEVERSION
                </p>
      </td>
      <td>
        <p caps_internal_Id="01f8de2d-2923-40ed-819d-9747bbc55fc9">
                    The latest version control change that is included in this build. This will be version control specific based on the provider. For Git this will the commit ID and for Tfvc this will be the change set number in the format CS1234.
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="f9893a9d-43a0-475b-a912-6c4aa3b60cf6">
                    Build.SourceBranch
                </p>
      </td>
      <td>
        <p caps_internal_Id="b82795a8-62ad-45cb-92d1-bcd7aee24f1f">
                    BUILD_SOURCEBRANCH
                </p>
      </td>
      <td>
        <p caps_internal_Id="67f60853-de26-49a9-8695-54f54e5b4a29">
                    The branch the build was queued for.
                </p>
        <ul>
          <li caps_internal_Id="5e69947d-7f5a-41dd-9eca-31bbc02adc28">For Git this is the name of the branch in the formation `refs/heads/master`.</li>
          <li caps_internal_Id="d3c8b66c-44f8-4529-9cc4-3bddf3fd0acc">For Tfvc this will be the root server path for the workspace. `$/teamproject/branch`</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="a31e806f-4c6a-4206-a402-010f24c66a32">
                    Build.SourceBranchName
                </p>
      </td>
      <td>
        <p caps_internal_Id="59e68352-6eff-41f4-9a7f-98b3e955c443">
                    BUILD_SOURCEBRANCHNAME
                </p>
      </td>
      <td>
        <p caps_internal_Id="999f3a5f-82f8-40eb-b3ad-c85ee452a73d">
                    The name of the branch the build was queued for.
                </p>
        <ul>
          <li caps_internal_Id="75897ae8-b49c-49fa-946c-7eb8296ba024">For Git this is the last path segment in the ref. For example in `refs/heads/master` the name is `master`.</li>
          <li caps_internal_Id="5e55bd8a-969a-4042-a03c-afc09a0cf71d">For Tfvc this will last path segment in the root server path for the workspace. For example in `$/teamproject/branch` the name is `branch`</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="74948b5d-2dcc-4565-90eb-5e9d1b1b56c5">
                    Build.Repository.Name
                </p>
      </td>
      <td>
        <p caps_internal_Id="fbc97dce-1cff-455b-a0ef-1de3785d40b4">
                    BUILD_REPOSITORY_NAME
                </p>
      </td>
      <td>
        <p caps_internal_Id="0013e3f4-e3d5-4887-900c-571f0e43bfeb">
                    The name of the repository in the build. By default this is the name of the Git repository or the name of the team project in the case of Tfvc.
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="cac8941d-41cc-4d9e-a0e1-4f87b7c9297a">
                    Build.Repository.Provider
                </p>
      </td>
      <td>
        <p caps_internal_Id="9b87b3f2-fac2-415d-9f6d-292e27b0ac5b">
                    BUILD_REPOSITORY_PROVIDER
                </p>
      </td>
      <td>
        <p caps_internal_Id="06be0d9f-94e2-4a54-a96a-5021e3cd50f6">
                    The name of the source provider for the build
                </p>
        <ul>
          <li caps_internal_Id="8f9c172d-76bc-4286-83fa-fbb1baea6b5f">TfGit - Git repository hosted in the TFS server</li>
          <li caps_internal_Id="a6222b19-8290-43b4-b246-c8e872698bc4">Tfvc - Team Foundation Version Control</li>
          <li caps_internal_Id="a5d31ec5-2e02-4891-92b3-b7401cc44d0b">Git - Git repository hosted on an external server</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="5f0cd519-f852-4c91-93c9-e46c370577b9">
                    Build.Repository.Clean
                </p>
      </td>
      <td>
        <p caps_internal_Id="eb7b555e-6b28-4800-802b-7203dbcd92f6">
                    BUILD_REPOSITORY_CLEAN
                </p>
      </td>
      <td>
        <p caps_internal_Id="e10bf3ad-ba25-4ad7-85f1-5ec979c82f86">
                    Whether or not to perform an incremental update of the local copy of the sources. In the case of Git we will delete the checkout files always and only delete the .git folder if this is set to true
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="ce87c4cd-8d86-4ccf-87dd-2cc3ff7f8fcc">
                    Build.Repository.Uri
                </p>
      </td>
      <td>
        <p caps_internal_Id="4da93a7d-1991-4e5b-b4da-67e92c6be39f">
                    BUILD_REPOSITORY_URI
                </p>
      </td>
      <td>
        <p caps_internal_Id="89aa7e72-cce7-4182-a9ae-adcbb5b0c67b">
                    The URL for the repository synced for this build
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="b8d8a767-45d7-4a5c-abe1-d02fff91d88b">
                    Build.Repository.Tfvc.Workspace
                </p>
      </td>
      <td>
        <p caps_internal_Id="a75db4ad-f351-4553-9290-d2d23086a99c">
                    BUILD_REPOSITORY_TFVC_WORKSPACE
                </p>
      </td>
      <td>
        <p caps_internal_Id="3cf46e69-16bf-4439-bc39-bbdf8b047c31">
                    The name of the workspace created for the build.This is only valid for Tfvc.
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="4a51ea85-be42-4c43-9735-855d51a0d7b3">
                    Build.Repository.Tfvc.Shelveset
                </p>
      </td>
      <td>
        <p caps_internal_Id="4ac6d1e4-35fc-490f-be09-b248997e672a">
                    BUILD_REPOSITORY_TFVC_SHELVESET
                </p>
      </td>
      <td>
        <p caps_internal_Id="d20a04ab-5b11-44b1-9503-218a1dcdc9aa">
                    The name of the shelveset to include in this build. This is used for buddy build and code review validation scnarios
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="b46a5c6d-7d9f-4792-b7e5-247106e69d0d">
                    Build.Repository.Git.SubmoduleCheckout
                </p>
      </td>
      <td>
        <p caps_internal_Id="f5177220-2663-4447-b0e6-8ceda1f6d575">
                    BUILD_REPOSITORY_GIT_SUBMODULECHECKOUT
                </p>
      </td>
      <td>
        <p caps_internal_Id="27045be1-9a3e-43d1-ae7e-2c802e2698dc">
                    Whether or not to checkout submodules.
                </p>
      </td>
    </tr>
  </tbody>
</table>

####Agent Variables

<table>
  <thead>
    <tr>
      <th caps_internal_Id="0702008e-ff34-4287-8d0c-85ab48e0677a">Variable Name</th>
      <th caps_internal_Id="3641eac4-d9b2-4049-9be0-4eb68a016fa4">Environment Variable Name</th>
      <th caps_internal_Id="0ff6b3c1-fe88-43b4-9016-ecf106c47b3f">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p caps_internal_Id="7389b8b7-c3b5-4cbb-96bc-0dde0bcf86dd">
                    Agent.Name
                </p>
      </td>
      <td>
        <p caps_internal_Id="a8581291-5bca-487e-be36-6a486b6e8dec">
                    AGENT_NAME
                </p>
      </td>
      <td>
        <p caps_internal_Id="57231179-7073-43fd-92b7-6e95d2ad4ec3">
                    The name of the agent as it is registered with the pool. This is likely different than the machine name
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="d85f8641-da71-438e-aaf2-06d6542f6a2e">
                    Agent.Id
                </p>
      </td>
      <td>
        <p caps_internal_Id="cf15ae96-4b99-470d-bcd4-a0c5d02c7f34">
                    AGENT_ID
                </p>
      </td>
      <td>
        <p caps_internal_Id="b4b697f2-1f50-403f-ae83-9418795bff87">
                    The ID of the agent.  
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="1b9db8e2-536e-4e72-9356-d951377cfb84">
                    Agent.HomeDirectory
                </p>
      </td>
      <td>
        <p caps_internal_Id="98aa5fa7-ae67-4f5a-8c4d-ca6993d2af93">
                    AGENT_HOMEDIRECTORY
                </p>
      </td>
      <td>
        <p caps_internal_Id="654455da-f5db-4770-af23-f55490a86ef6">
                    The directory the agent is installed into. This contains the agent bits
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="a36b96ad-8193-4e0f-9478-f2111554469d">
                    Agent.RootDirectory
                </p>
      </td>
      <td>
        <p caps_internal_Id="7d495e3d-6c24-438c-bb30-efdfee0a05ae">
                    AGENT_ROOTDIRECTORY
                </p>
      </td>
      <td>
        <p caps_internal_Id="ab4005ed-0aba-4c73-b598-6dd967eae1f6">
                    The root directory for this agent to sync source. By default it is the same as `Agent.HomeDirectory`
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="02e6520c-76bc-43f8-8745-23d8fe0887ec">
                    Agent.WorkFolder
                </p>
      </td>
      <td>
        <p caps_internal_Id="44e2d882-c28d-4056-9362-00937a1b7d1c">
                    AGENT_WorkFolder
                </p>
      </td>
      <td>
        <p caps_internal_Id="db4827f2-4be2-4688-8f92-dd6e419865cb">
                    The working directory for this agent. By default `$(Agent.RootDirectory)\_work`.
                </p>
      </td>
    </tr>
  </tbody>
</table>

##Agent Build Variables

<table>
  <thead>
    <tr>
      <th caps_internal_Id="193b43d1-2c63-454b-b497-7b7e13ed1a38">Variable Name</th>
      <th caps_internal_Id="7cc9dcf7-3dc3-4f9a-b20b-552f4c17aa8f">Environment Variable Name</th>
      <th caps_internal_Id="8f6b4120-0d6f-4574-871b-701daebb35fb">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p caps_internal_Id="44fea5b9-14cb-4982-b255-f7eebe59f29c">
                    Build.Repository.LocalPath
                </p>
      </td>
      <td>
        <p caps_internal_Id="84edb495-3811-4792-a6f6-7ebf65c99ac0">
                    BUILD_REPOSITORY_LOCALPATH
                </p>
      </td>
      <td>
        <p caps_internal_Id="98b0d78d-2ab5-439b-9c8e-0fb092e995e3">
                    The local path on the agent where the sources are syncd to. Will be `$(Agent.BuildDirectory)\$(Build.Repository.Name)`
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="3ee5f8e7-5648-4798-bf82-a822313d1f08">
                    Build.SourcesDirectory
                </p>
      </td>
      <td>
        <p caps_internal_Id="e2f12535-56e4-44e9-8398-8ef0cc9bf2d9">
                    BUILD_SOURCESDIRECTORY
                </p>
      </td>
      <td>
        <p caps_internal_Id="d5b700cc-525f-478b-8b84-63b95e1f2e89">
                    The folder where all sources for the build definition are synced to.Will be `$(Agent.BuildDirectory)\$(Build.Repository.Name)`
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="0aa12315-02bd-485c-8395-720b60b398d3">
                    Build.ArtifactsStagingDirecotry
                </p>
      </td>
      <td>
        <p caps_internal_Id="b6b599ad-387c-4854-98ec-efce8161a075">
                    BUILD_ARTIFACTSTAGINGDIRECTORY
                </p>
      </td>
      <td>
        <p caps_internal_Id="3e96aad2-f079-40b5-bf5a-209ca14b9e14">
                    The local path on the agent where any artifacts are copied to before being pushed to their destination, it will be cleaned up after the files of been copied to the destination. Will be `$(Agent.BuildDirectory)\Artifacts`
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="ad86244f-e68b-4121-9d93-b512a8dd14e2">
                    Build.StagingDirecotry
                </p>
      </td>
      <td>
        <p caps_internal_Id="9f1ba40c-3609-4f3d-a6db-27f6c1c2fb10">
                    BUILD_STAGINGDIRECTORY
                </p>
      </td>
      <td>
        <p caps_internal_Id="1c4d47e5-2505-4b53-b52e-b3fc1fe37d8d">
                    Wellknown directory on disk where you can place build outputs or other temporary data, it will be cleand up between builds. It is `$(Agent.BuildDirectory)\Staging`
                </p>
      </td>
    </tr>
    <tr>
      <td>
        <p caps_internal_Id="765ad1dc-d041-4fc3-902e-e2e53983dac5">
                    Agent.BuildDirectory
                </p>
      </td>
      <td>
        <p caps_internal_Id="cdbf12ff-8eae-4c62-a4af-5029685e1072">
                    AGENT_BUILDDIRECTORY
                </p>
      </td>
      <td>
        <p caps_internal_Id="adea9e4e-4635-446f-9d7b-653a196545b5">
                    The local path on the agent where all folders for a given build definition are created. Will be $(Agent.RootDirectory\hash of collection of definitionid and repository URL
                </p>
      </td>
    </tr>
  </tbody>
</table>

