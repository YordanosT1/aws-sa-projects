Guided Lab: Exploring AWS Identity and Access Management (IAM)
#
aws
#
iam
#
lab
#
cloudcomputing
Lab overview
In this lab, you explore users and groups and inspect the associated policies in the AWS Identity and Access Management (IAM) service. You also add users to the groups and verify the permissions that are inherited by them.

Objective of the lab is to:

Explore pre-created IAM users and groups.
Inspect IAM policies as they were applied to the pre-created groups.
Follow a real-world scenario, while adding users to groups with specific capabilities enabled.
Locate and use the IAM sign-in URL.
Test the effects of policies on service access.
Task 1: Explore the users and groups, and inspect policies
In this task, I explored the users and groups that were created for you in IAM.
Opened AWS Console and selected IAM from services.
Exploring Users
For this lab, there are already 3 users that were already created; user-1, user-2 and user-3.


Exploring User 1 and my observations:

1 From the tab. There are 0 permissions policies. This means that user-1 does not have permissions.
 

2 From the tab, I observed that user-1 is not a member of any groups.



3 From the tab, observed that user-1 is assigned a Console password. This allows the user to access the AWS Management Console.



Exploring Groups
For this lab, there are already 3 groups that were already created; EC2-Admin, EC2-Support, and S3-Support
Exploring group the group < EC2-Support > and my observations:
1 From the tab the group has a managed policy, AmazonEC2ReadOnlyAccess.

 

2 After navigating the expand button on , there is a policy written in JSON defining the permissions. The description displays that the following “Provides read only access to Amazon EC2 via the AWS Management Console.”
a. The permission allows the members of the group to describe (view) EC2, ELB (Elastic Load Balancing, and EC2 Auto Scaling. They also have permissions to list and describe (view) CloudWatch. From the permission policy, I can also understand that the members of groups are not granted permission to modify the resources they are allowed to view. NB This type of permission policy is a good fit for assigning to a support role.

b. From the structure of the policy I observed that IAM has 3 basic structures. Effect, Action and resources.
i. Effect defines whether the permission allows or delays.
ii. Action specifies the API calls that can be made against an AWS service (for example, cloudwatch:ListMetrics). Defines the permission action like permission to describe, permission to list and permission to modify.
iii. Resource defines the scope of entities covered by the policy rule (for example, a specific Amazon Simple Storage Service [Amazon S3] bucket or Amazon EC2 instance; an asterisk [ * ] means any resource).

 

The policy (JSON code):
{
"Version": "2012-10-17",
"Statement": [
{
"Effect": "Allow",
"Action": [
"ec2:Describe*",
"ec2:GetSecurityGroupsForVpc"
],
"Resource": ""
},
{
"Effect": "Allow",
"Action": "elasticloadbalancing:Describe",
"Resource": ""
},
{
"Effect": "Allow",
"Action": [
"cloudwatch:ListMetrics",
"cloudwatch:GetMetricStatistics",
"cloudwatch:Describe"
],
"Resource": ""
},
{
"Effect": "Allow",
"Action": "autoscaling:Describe",
"Resource": "*"
}
]
}

Exploring group the group < S3-Support > and my observations:
1 From the tab the group has a managed policy, AmazonS3ReadOnlyAccess.

 
2 After navigating the expand button on < AmazonS3ReadOnlyAccess >, there is a policy written JSON defining the permissions. The description displays that the following “Provides read only access to all buckets via the AWS Management Console.”
A The policy allows the members of the group to get, list, and describe (view) all resources in the S3 bucket.
** The policy (JSON code):**
{
"Version": "2012-10-17",
"Statement": [
{
"Effect": "Allow",
"Action": [
"s3:Get*",
"s3:List*",
"s3:Describe*",
"s3-object-lambda:Get*",
"s3-object-lambda:List*"
],
"Resource": "*"
}
]
}

Exploring group the group < EC2-Admin > and my observations:

From the tab the group has a managed policy, EC2-Admin-Policy.
 
2 After navigating the expand button on < EC2-Admin-Policy>, there is a policy written in JSON defining the permissions.
a The policy allows the members of the group to describe (view), to start and stop EC2 instances.
{
"Version": "2012-10-17",
"Statement": [
{
"Condition": {
"ForAllValues:StringLikeIfExists": {
"ec2:InstanceType": [
".nano",
".micro"
]
}
},
"Action": [
"ec2:Describe*",
"ec2:StartInstances",
"ec2:StopInstances"
],
"Resource": [
"*"
],
"Effect": "Allow"
}
]
}

Business scenario
For the remainder of this lab, you work with these users and groups to enable permissions that support the following business scenario.

Your company is growing its use of AWS services and is using many Amazon EC2 instances and Amazon S3 buckets. You want to give access to new staff based on their job function, as indicated in the following table.
 

Task 2: Add users to groups
Business need 1
You recently hired user-1 into a role where they will provide support for Amazon S3. In this task, you add them to the S3-Support group so that they inherit the necessary permissions through the attached AmazonS3ReadOnlyAccess policy.

My Observation
There are no users added to the S3-Support group.

Actions taken
To meet the business need I added user-1 to the group.


Business need 2
You hired user-2 into a role where they will provide support for Amazon EC2. You will add them to the EC2-Support group so that they inherit the necessary permissions through the attached AmazonEC2ReadOnlyAccess policy.

My Observation
There are no users added to the EC2-Support group.


Actions taken
To meet the business need I added user-2 to the group.


Business need 3
You hired user-3 as your Amazon EC2 administrator to manage your EC2 instances. You will add them to the EC2-Admin group so that they inherit the necessary permissions through the attached EC2-Admin-Policy.

My Observation
There are no users added to the EC2-Admin group.


Actions taken
To meet the business need I added user-3 to the group.

Navigating to and my observation
Each groups have a user indicated as <1> in each raw under the column Users as seen in the picture below.


Task 3: Sign in and test user permissions
This part of the lab let me test the permissions inherited by IAM users in the console.
Signing in as user-1
Navigating to , My observations and Actions taken


My observation before signing in as user-1
On the right side of the page there Sign-in URL for IAM users in this account section at the top of the page. It is used to sign in to the AWS account that you are currently using.

Actions taken

• Opened private tab on a browser
• Copied and pasted the sign in URL. And by doing so I am able to sign in as whom I hired as my Amazon S3 storage support staff.
• I signed in by using the credentials given in the lab instructions.


My observation after signing in as user-1
• Navigating to S3
o I am able to view S3 bucket


My observation after signing in as user-1
• Navigating to EC2
o I am not able to access nor view EC2 instances.
o There is an error message displayed as well. “You are not authorized to perform this operation.”


Signing in as user-2
Actions taken to sign in as user-2
After signing out as I went to my console:
• Opened private tab on a browser
• Copied and pasted the sign in URL. And by doing so I am able to sign in as whom I hired as my Amazon EC2 support staff.
• I signed in by using the credentials given in the lab instructions.


My observation after signing in as user-2
• Navigating to EC2 and attempting to view EC2 instances
o I am able to view EC2 instances. And from my observation there were no EC2 instances deployed in the region.
o There is an instance deployed in the region.


• Navigating to the EC2 and attempting stop an instance
o Actions taken
- Navigated to instances
- Selected the instance in the region.
- Went to actions and selected
o Signed in as user-2, I am not able to stop the instance. An error occurs after I attempt to stop the instance.


My observation after signing in as user-2
• Navigating to S3
o I am not able to access nor view S3 buckets.
o There is an error message displayed as well. “Access denied.”


Signing in as user-3
Actions taken to sign in as user-3
After signing out as I went to my console:
• Opened private tab on a browser
• Copied and pasted the sign in URL. And by doing so I am able to sign in as whom I hired to manage your EC2 instances.
• I signed in by using the credentials given in the lab instructions.


My observation after signing in as user-3
• Navigating to EC2 and attempting to view EC2 instances
o I am able to view EC2 instances. And from my observation there were no EC2 instances deployed in the region.
o There is an instance deployed in the region.


• Attempting to start and stop an instance
o Actions taken
- Navigated to instances
- Selected the instance in the region.
- Went to actions and selected
o Signed in as user-3, I am able to stop the instance.

What I have learnt
• Explored pre-created IAM users and groups
• Inspected IAM policies as applied to the pre-created groups
• Followed a real-world scenario, while adding users to groups with specific capabilities enabled
• Located and used the IAM sign-in URL
• Tested the effects of policies on service access
