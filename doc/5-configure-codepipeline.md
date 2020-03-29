# Configure CodePipeline

## Description

In this section we will put all together by creating a pipeline to automate all the process
 
---

## Configure CodeDeploy

 - Browse to "Pipelines" (under Pipeline) in the CodePipeline portal and click on "Create pipeline"

 <kbd>
   <img src="/doc/images/05-01-codepipeline-01.png" width="800">
 </kbd>

 - Configure the pipeline settings with the details below:
   - Pipeline name: bluegreen-pipeline
   - Service Role: New service role
   - Role name: bluegreen-codepipeline-role

 <kbd>
   <img src="/doc/images/05-01-codepipeline-02.png" width="800">
 </kbd>

 - Under "Source" configure the pipeline to retrieve the source code from your github repository and use github webhooks

 <kbd>
   <img src="/doc/images/05-01-codepipeline-03.png" width="800">
 </kbd>

 - Set the build provider as CodeBuild and use the project "bluegreen-build"

 <kbd>
   <img src="/doc/images/05-01-codepipeline-04.png" width="800">
 </kbd>

 - Configure the deployment to use the application "bluegreen-deploy" and the group "bluegreen-deploygroup"

 <kbd>
   <img src="/doc/images/05-01-codepipeline-05.png" width="800">
 </kbd>

 - Review the pipeline configuration and click on "Create pipeline"

 <kbd>
   <img src="/doc/images/05-01-codepipeline-06.png" width="800">
 </kbd>

 - Finally, the pipeline will be automatically triggered

 <kbd>
   <img src="/doc/images/05-01-codepipeline-07.png" width="800">
 </kbd>

 - Wait until the pipeline finish and browse to the load balancer dns (note that the build id appear in the page)

 <kbd>
   <img src="/doc/images/05-01-codepipeline-08.png" width="800">
 </kbd>
