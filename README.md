# Blue/Green Deployment for Autoscaling Groups with CodePipeline, CodeBuild and CodeDeploy

## In this tutorial:

In this tutorial we will create an initial environment composed by an autoscaling group with two ubuntu machines behind a load balancer, then we will create a CI/CD pipeline to provide blue/green deployments using CodeBuild, CodeDeploy and CodePipeline. 

 1. [Prerequisites](/doc/1-prerequisites.md)
 2. [Setup initial environment](/doc/2-setup-initial-environment.md)
 3. [Configure CodeBuild](/doc/3-configure-codebuild.md)
 4. [Configure CodeDeploy](/doc/4-configure-codedeploy.md)
 5. [Configure CodePipeline](/doc/5-configure-codepipeline.md)
 6. [Putting all together](/doc/6-putting-all-together.md)
 
## The benefits of blue/green deployments

Blue/green deployment involves two production environments:

 - Blue is the active environment.
 - Green is for the release of a new version.

<kbd>
  <img src="/doc/images/00-00-blue-green-ec2-01.png" width="600">
</kbd>
 
## Here are some of the advantages of a blue/green deployment:

 - You can perform testing on the green environment without disrupting the blue environment.
 - Switching to the green environment involves no downtime. It only requires the redirecting of user traffic.
 - Rolling back from the green environment to the blue environment in the event of a problem is easier because you can redirect traffic to the blue environment without having to rebuild it.
 - You can incorporate the principle of infrastructure immutability by provisioning fresh instances when you need to make changes. In this way, you avoid configuration drift.

## AWS CodeDeploy offers two ways to perform blue/green deployments:

 - In the first approach, AWS CodeDeploy makes a copy of an Auto Scaling group. It, in turn, provisions new Amazon EC2 instances, deploys the application to these new instances, and then redirects traffic to the newly deployed code.
 - In the second approach, you use instance tags or an Auto Scaling group to select the instances that will be used for the green environment. AWS CodeDeploy then deploys the code to the tagged instances.

## References

 - https://docs.aws.amazon.com/codebuild/latest/userguide
 - https://docs.aws.amazon.com/codedeploy/latest/userguide
 - https://docs.aws.amazon.com/codepipeline/latest/userguide
