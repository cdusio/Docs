.. meta::
   :description: Aviatrix Support Center
   :keywords: Aviatrix, Support, Support Center

===========================================================================
Controller
===========================================================================

What are the minimum requirements for an instance to run the Aviatrix Controller Software?
---------------------------------------------------------------------------------------------------

We strongly recommend that the instance be at least t2.large and have at least 20GB of Storage to act as a Controller in AWS. Please check out https://docs.aviatrix.com/StartUpGuides/aviatrix-cloud-controller-startup-guide.html#select-instance-size for more information.
 
If you have less than 20GB of Storage on your controller, please follow these steps to increase your disk space:

1. Make a backup of your controller. (https://docs.aviatrix.com/HowTos/controller_backup.html)
2. Login to AWS console and locate the Aviatrix controller instance.
3. Click on Root device: /dev/sda1 and then click on EBS ID vol-xxxxxxxxxx.
4. With the volume selected, click Action > Modify Volume to change the Size 20.
5. Click OK to start the resize process. Please make sure you wait until the State of the volume is "in-use - completed (100%)".
6. Select the Aviatrix controller instance in EC2 page. Click Reboot for the disk space to take effect.
7. Confirm the controller is in running state in AWS console.
8. Login to your controller to sanity test.
9. Take a backup again, by following instructions at https://docs.aviatrix.com/HowTos/controller_backup.html

Note that rebooting the controller will not impact your IPsec tunnels as it's not in the data path. Please send email to support@aviatrix.com, if you have any question.



Why are IAM policies important?
---------------------------------

During the launch of your Aviatrix Controller, two IAM roles(aviatrix-role-ec2 & aviatrix-role-app) are created and two associated IAM policies(aviatrix-assume-role-policy & aviatrix-app-policy) are also created. These roles and policies allow the Controller to use AWS APIs to launch gateway instances, create new route entries and build networks and hence very important to keep your network operational. Please check out `IAM Policies <https://docs.aviatrix.com/HowTos/iam_policies.html>`_, `Requirements <https://docs.aviatrix.com/HowTos/aviatrix_iam_policy_requirements.html>`_, `Customization <https://docs.aviatrix.com/HowTos/customize_aws_iam_policy.html>`_ and `IAM for Secondary Access Accounts <https://docs.aviatrix.com/HowTos/HowTo_IAM_role.html?highlight=iam>`_. After a software upgrade, please do update your IAM policies using the instructions in the above links - these updates have to be done for all accounts that have the Controller and the gateway. 


Why should I upgrade my Controller Software?
----------------------------------------------

Our engineering team works very hard to fix issues on a continuous basis. We also continue to add new features and try to enhance the systems to keep your network working effectively and securely. Please take advantage of this and stay on the latest releases.  `Controller upgrade <https://docs.aviatrix.com/HowTos/inline_upgrade.html>`_ does not affect your tunnels. Please keep the your controller's size at > t2.large and please don't encrypt the root devices!!


Does Aviatrix Controller support automation?
-------------------------------------------------

Aviatrix Controller supports a `comprehensive set of REST API <https://s3-us-west-2.amazonaws.com/avx-apidoc/index.htm>`_ to enable automation

We also support Terraform. Please check out `Aviatrix Terraform Tutorial <https://docs.aviatrix.com/HowTos/tf_aviatrix_howto.html>`_, `Aviatrix Terraform Provider <https://docs.aviatrix.com/HowTos/aviatrix_terraform.html>`_, `Transit Network using Terraform <https://docs.aviatrix.com/HowTos/Setup_Transit_Network_Terraform.html>`_ and our `Github Repository <https://github.com/AviatrixSystems/terraform-provider-aviatrix>`_.


Can I use an SSL Certificate from AWS ACM?
-------------------------------------------

You can place your `controller behind an ELB in AWS <https://docs.aviatrix.com/HowTos/controller_ssl_using_elb.html>`_ and use your certificate from AWS ACM. Remember to increase the `default ELB idle connection timeout <https://docs.aws.amazon.com/elasticloadbalancing/latest/application/application-load-balancers.html#connection-idle-timeout>`_ from 60 seconds to at least 300 seconds.


How do I backup my Aviatrix configuration?
------------------------------------------

Please checkout `backup functionality <https://docs.aviatrix.com/HowTos/controller_backup.html>`_ on your Aviatrix controller. 

* If you have a "."/period character in the S3 bucket name, please ensure you are running software version 4.0.685 or later.)
* We strongly recommend the "Multiple Backup" setting to be turned Controller/Settings/Maintenance/Backup&Restore. After turning this option - click on Disable and then Enable and then click on "Backup Now" and check in your S3 bucket to make sur e that the backup function is successful.
* We support `backup using AWS encrypted storage <https://docs.aviatrix.com/HowTos/controller_backup.html?highlight=backup%20restore#how-to-backup-configuration-with-aws-encrypted-storage>`_
* Please do not use AWS's AMI to take snapshots - this is not a valid backup mechanism and will not work


How can I customize Controller GUI?
--------------------------------------

* On the Gateway page, you can customize the columns and add more information(click on the "Name, State, ..." drop down list box and select fields you are interested in). You can also sort and filter on any column by clicking on header.
* On the gateay page, you can adjust the number of gateways you can see at a time - the default is 5 gateways

How can I troubleshoot connectivity issues?
--------------------------------------------
Please refer to `How to use Aviatrix FlightPath <https://docs.aviatrix.com/HowTos/flightpath_deployment_guide.html>`_!! For more details, please check out  `Aviatrix Flightpath <https://docs.aviatrix.com/HowTos/flightpath_deployment_guide.html>`_!!


Does Aviatrix support High Availability?
------------------------------------------

We have HA built into our system through `Transit HA <https://docs.aviatrix.com/HowTos/transitvpc_workflow.html>`_ and `Single AZ HA <https://docs.aviatrix.com/HowTos/gateway.html#gateway-single-az-ha>`_. The `Gateway HA <https://docs.aviatrix.com/Solutions/gateway_ha.html>`_ is now deprecated. 

`Aviatrix Controller HA <https://docs.aviatrix.com/HowTos/controller_ha.html>`_ does not support HA in multiple regions, but works across multiple AZ's. More information `here <https://github.com/AviatrixSystems/Controller-HA-for-AWS/blob/master/README.md>`_


Does Controller send alerts when Gateway status changes?
--------------------------------------------------------------------

Aviatrix Controller monitors the gateways and tunnels and whenever there is a tunnel or gateway state change, it will send an email to the admin of the system. You can always override the admin email by updating "ControllerUi/Settings/Controller/Email/StatusChangeEventEmail". If you do not want to see these emails, you can set it to an email address that you don't monitor.

As an alternative, you can also set Cloudwatch Event Alerts in AWS to be alerted when Gateway/Controller Instances are Started or Stopped.

