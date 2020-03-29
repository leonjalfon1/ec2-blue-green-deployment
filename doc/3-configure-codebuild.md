# Configure CodeBuild

## Description

In this section we will configure a CodeBuild project that will update the "index.html" to add the build Id
 
---

## Configure a CodeBuild Project

 - Browse to "Build projects" (under Build) in the CodeBuild portal and click on "Create build project"

 <kbd>
   <img src="/doc/images/03-01-codebuild-01.png" width="800">
 </kbd>

 - In the "Project configuration" name the build as "bluegreen-build"

 <kbd>
   <img src="/doc/images/03-01-codebuild-02.png" width="800">
 </kbd>

 - In the "Source" section set the following details
   - Source provider: GitHub
   - Repository: Repository in my GitHub account
   - Github repository: <github-username>/<repository-name>
   - Source version: master

 <kbd>
   <img src="/doc/images/03-01-codebuild-03.png" width="800">
 </kbd>

 - Under "Primary source webhook events" set:
   - Enable checkbox "Rebuild every time a code change is pushed to this repository"
   - Event type: PUSH

 <kbd>
   <img src="/doc/images/03-01-codebuild-04.png" width="800">
 </kbd>

 - Configure the "Environment" section with the following details:
   - Environment Image: Managed image
   - Operating system: Ubuntu
   - Runtimes: Standard
   - Image: aws/codebuild/standard:4.0
   - Image version: Always use the latest image for this runtime version
   - Environment type: Linux
   - Service role: Existing service role
   - Role ARN: select "bluegreen-codebuild-role"

 <kbd>
   <img src="/doc/images/03-01-codebuild-05.png" width="800">
 </kbd>

 - Under "Buildspec" choose "Use a buildspec file"

 <kbd>
   <img src="/doc/images/03-01-codebuild-06.png" width="800">
 </kbd>

 - In the "Artifacts" section set the type as "No artifacts"

 <kbd>
   <img src="/doc/images/03-01-codebuild-07.png" width="800">
 </kbd>

 - Under "Logs" keep enable the CloudWatch logs and set the following parameters:
   - Group name: bluegreen
   - Stream name: codebuild

 <kbd>
   <img src="/doc/images/03-01-codebuild-08.png" width="800">
 </kbd>

 - Finally test the build by click on "Start build" (keep default values)

 <kbd>
   <img src="/doc/images/03-01-codebuild-09.png" width="800">
 </kbd>

 - You will be redirected to the build log page

 <kbd>
   <img src="/doc/images/03-01-codebuild-10.png" width="800">
 </kbd>