# Setup Initial Environment

## Description

In this section we will setup our initial environment by configuring:

 1. Launch Configuration
 2. Autoscaling Group
 3. Target Group
 4. Load Balancer
 5. Configure Servers
 
---

## Create a Launch Configuration

 - Browse to "Launch Configurations" (under Auto Scaling) in the EC2 portal

 <kbd>
   <img src="/doc/images/02-01-launchconfiguration-01.png" width="800">
 </kbd>

 - Then click to "Create launch configuration"

 <kbd>
   <img src="/doc/images/02-01-launchconfiguration-02.png" width="800">
 </kbd>

 - Select "Ubuntu 18.04" as the AMI template for the instances

 <kbd>
   <img src="/doc/images/02-01-launchconfiguration-03.png" width="800">
 </kbd>

 - Select "t2.micro" as the instance type

 <kbd>
   <img src="/doc/images/02-01-launchconfiguration-04.png" width="800">
 </kbd>

 - Configure the launch configuration with the following details and click on "Next: Add storage":
   - Name: bluegreen-launchconfig
   - IAM Role: bluegreen-ec2-role
   - Advanced -> User data:
     - Set a startup script for the instances to install the codedeploy agent
     ```
     #!/bin/bash
     apt-get -y update
     apt-get -y install ruby
     apt-get -y install wget
     cd /home/ubuntu
     wget https://aws-codedeploy-ap-southeast-1.s3.amazonaws.com/latest/install
     chmod +x ./install
     ./install auto
     systemctl enable codedeploy-agent
     systemctl start codedeploy-agent
     systemctl restart codedeploy-agent
     ```
     - Note: you may require update the script according to your region (I am working on "ap-southeast-1", for more details see: https://docs.aws.amazon.com/codedeploy/latest/userguide/resource-kit.html#resource-kit-bucket-names)
   - Advanced -> IP Address Type:
     - Assign a public IP address to every instance
 <kbd>
   <img src="/doc/images/02-01-launchconfiguration-05.png" width="800">
 </kbd>

 - Then you will be asked for the storage configuration, keep the defaults and click "Next: Configure Security Group"

 <kbd>
   <img src="/doc/images/02-01-launchconfiguration-06.png" width="800">
 </kbd>

 - Choose the security group that you created before (bluegreen-sg) and click "Review"

 <kbd>
   <img src="/doc/images/02-01-launchconfiguration-07.png" width="800">
 </kbd>

 - Review the details and click on "Create launch configuration"

 <kbd>
   <img src="/doc/images/02-01-launchconfiguration-08.png" width="800">
 </kbd>

 - Choose the SSH key pair created before (bluegreen-key), check the confirmation box and click on "Create launch configuration"

 <kbd>
   <img src="/doc/images/02-01-launchconfiguration-09.png" width="800">
 </kbd>

 - Finally in the confirmation page click on "Create an Auto Scaling group using this launch configuration"

 <kbd>
   <img src="/doc/images/02-01-launchconfiguration-10.png" width="800">
 </kbd>

---

## Configure an Autoscaling Group

 - Configure the autoscaling group with the parameters below and click on "Next: Configure scaling policies"
   - Group Name: bluegreen-tg
   - Group Size: 2
   - Network: <vpc-to-deploy-servers>
   - Subnets: <choose-all-subnets>

 <kbd>
   <img src="/doc/images/02-02-autoscalinggroup-01.png" width="800">
 </kbd>

 - Choose "Keep this group at its initial size" (default) and click on "Next: Configure notifications"

 <kbd>
   <img src="/doc/images/02-02-autoscalinggroup-02.png" width="800">
 </kbd>

 - We don't need any notification so just click on "Next: Configure Tags"

 <kbd>
   <img src="/doc/images/02-02-autoscalinggroup-03.png" width="800">
 </kbd>

 - Set the following tags and click on "Review" (those tags will be added to the instances created by the autos caling group)
   - Name=bluegreen 
   - App=bluegreen

 <kbd>
   <img src="/doc/images/02-02-autoscalinggroup-04.png" width="800">
 </kbd>

 - Review the details and click on "Create Auto Scaling group"

 <kbd>
   <img src="/doc/images/02-02-autoscalinggroup-05.png" width="800">
 </kbd>

 - Browse to "Instances" under "EC2" to see how your servers are being created by the auto scaling group

 <kbd>
   <img src="/doc/images/02-02-autoscalinggroup-06.png" width="800">
 </kbd>

---

## Create a Target Group

 - Browse to "Target Groups" (under Load Balancing) in the EC2 portal

 <kbd>
   <img src="/doc/images/02-03-targetgroup-01.png" width="800">
 </kbd>

 - Click on "Create target group"

 <kbd>
   <img src="/doc/images/02-03-targetgroup-02.png" width="800">
 </kbd>

 - Configure the target group with the details below:
   - Name: bluegreen-tg
   - Target Type: Instance
   - Protocol: HTTP
   - Port: 80
   - VPC: <vpc-where-servers-are-deployed>

 <kbd>
   <img src="/doc/images/02-03-targetgroup-03.png" width="800">
 </kbd>

 - Select the created target group, open the tab "Targets" and click on "Edit" to add the instances to the target group

 <kbd>
   <img src="/doc/images/02-03-targetgroup-04.png" width="800">
 </kbd>

 - Search the instances created by the auto scaling group ("bluegreen") and register them in the target group 

 <kbd>
   <img src="/doc/images/02-03-targetgroup-05.png" width="800">
 </kbd>

---

## Setup a Load Balancer

 - Browse to "Load Balancers" under "Load Balancing" in the EC2 portal

 <kbd>
   <img src="/doc/images/02-04-loadbalancer-01.png" width="800">
 </kbd>

 - Click on "Create Load Balancer"

 <kbd>
   <img src="/doc/images/02-04-loadbalancer-02.png" width="800">
 </kbd>

 - Let's create an Application Load Balancer by choose "HTTP/HTTPS"

 <kbd>
   <img src="/doc/images/02-04-loadbalancer-03.png" width="800">
 </kbd>

 - Configure the load balancer with the details below, then click on "Next: Configure Security Settings"
   - Name: bluegreen-lb
   - Scheme (default): internet-facing
   - IP adress type (default): ipv4
   - Listeners (default): HTTP-80
   - Availability Zones
     - VPC: <vpc-where-servers-are-deployed>
     - Subnets: <select-all>

 <kbd>
   <img src="/doc/images/02-04-loadbalancer-04.png" width="800">
 </kbd>

 - Click on "Next: Configure Security Groups"

 <kbd>
   <img src="/doc/images/02-04-loadbalancer-05.png" width="800">
 </kbd>

 - Select the security group "bluegreen-sg" and click on "Next: Configure Routing"

 <kbd>
   <img src="/doc/images/02-04-loadbalancer-06.png" width="800">
 </kbd>

 - Configure routing with the parameters below, then click on "Next: Register Targets"
   - Target Group: Existing target group
   - Name: bluegreen-tg
   - Target Type: Instance
   - Protocol: HTTP
   - Port: 80

 <kbd>
   <img src="/doc/images/02-04-loadbalancer-07.png" width="800">
 </kbd>

 - Ensure that the instances are registred and click on "Next: Review"

 <kbd>
   <img src="/doc/images/02-04-loadbalancer-08.png" width="800">
 </kbd>

 - Review the load balancer details and click on "Create"

 <kbd>
   <img src="/doc/images/02-04-loadbalancer-09.png" width="800">
 </kbd>

---

## Configure Servers

 - Get the public ip of your instances in the EC2 portal

 <kbd>
   <img src="/doc/images/02-05-configureservers-01.png" width="800">
 </kbd>

 - Ensure that the downloaded ssh key have the right permissions by run

 ```
 chmod 400 ~/Downloads/leonj-bluegreen-key.pem
 ```

 - Access the first instance by using the command

 ```
 ssh ubuntu@<instance1-public-ip> -i ~/Downloads/leonj-bluegreen-key.pem
 ```

 - Ensure that the CodeDeploy agent is up and running

 ```
 systemctl status codedeploy-agent
 ```

 <kbd>
   <img src="/doc/images/02-05-configureservers-02.png" width="800">
 </kbd>

 - Use the command below to install Apache 

 ```
 sudo apt-get install -y apache2
 ```

 - Browse to the first server, you should see the Apache default page

 <kbd>
   <img src="/doc/images/02-05-configureservers-03.png" width="800">
 </kbd>

 - Exit from the first instance and access to the second one

 ```
 ssh ubuntu@<instance2-public-ip> -i ~/Downloads/leonj-bluegreen-key.pem
 ```

 - Use the command below to install Apache 

 ```
 sudo apt-get install -y apache2
 ```

 - Browse to the second server, you should see the Apache default page

 <kbd>
   <img src="/doc/images/02-05-configureservers-03.png" width="800">
 </kbd>

 - Browse to the load balancer to ensure that it's working as expected

 <kbd>
   <img src="/doc/images/02-05-configureservers-03.png" width="800">
 </kbd>
