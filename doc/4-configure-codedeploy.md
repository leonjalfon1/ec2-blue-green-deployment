# Configure CodeDeploy

## Description

In this section we will configure CodeDeploy to allow it manage our application
 
---

## Configure CodeDeploy

 - Browse to "Applications" (under Deploy) in the CodeDeploy portal and click on "Create application"

 <kbd>
   <img src="/doc/images/04-01-codedeploy-01.png" width="800">
 </kbd>

 - Name it "bluegreen-deploy" and set it to use "EC2/On-premises"

 <kbd>
   <img src="/doc/images/04-01-codedeploy-02.png" width="800">
 </kbd>

 - Click on "Create deployment group"

 <kbd>
   <img src="/doc/images/04-01-codedeploy-03.png" width="800">
 </kbd>

 - Set the deployment group name as "bluegreen-deploygroup"

 <kbd>
   <img src="/doc/images/04-01-codedeploy-04.png" width="800">
 </kbd>

 - Choose the IAM role "bluegreen-codedeploy-role"

 <kbd>
   <img src="/doc/images/04-01-codedeploy-05.png" width="800">
 </kbd>

 - Select "Blue/Green" as deployment type

 <kbd>
   <img src="/doc/images/04-01-codedeploy-06.png" width="800">
 </kbd>

 - Choose "Autmatically copy Amazon EC2 Auto Scaling group" and set the auto scaling group "bluegreen-asg"

 <kbd>
   <img src="/doc/images/04-01-codedeploy-07.png" width="800">
 </kbd>

 - Under deployment settings configure the below:
   - Reroute traffic immediately 
   - Terminate the original instances in the deployment group
   - Deployment configuration: CodeDeployDefault.AllAtOnce

 <kbd>
   <img src="/doc/images/04-01-codedeploy-08.png" width="800">
 </kbd>

 - Select "Application Load Balancer" and choose the target group "bluegreen-tg" and click on "Create deployment group"

 <kbd>
   <img src="/doc/images/04-01-codedeploy-09.png" width="800">
 </kbd>

 - Finally click on "Create deployment" to trigger a new deployment (manually)

 <kbd>
   <img src="/doc/images/04-01-codedeploy-10.png" width="800">
 </kbd>

 - Choose the deployment group, select the last github commit and trigger the deployment

 <kbd>
   <img src="/doc/images/04-01-codedeploy-11.png" width="800">
 </kbd>

 - Then you will be redirected to the deployment portal and you will be able to track the process

 <kbd>
   <img src="/doc/images/04-01-codedeploy-12.png" width="800">
 </kbd>

 - Wait until the deployment finish and browse to the load balancer dns

 <kbd>
   <img src="/doc/images/04-01-codedeploy-13.png" width="800">
 </kbd>
