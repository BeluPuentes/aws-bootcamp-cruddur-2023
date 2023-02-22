# Week 0 — Billing and Architecture

## Description
In the first week of the bootcamp we started using AWS, in order to do that we created a AWS Account. With all that set up, we started creating an Admin User, using CloudShell, generating AWS Credentials, installing AWS CLI and creating a Billing Alarm and a Budget.

## Create AWS account
![image](https://user-images.githubusercontent.com/93335543/219703084-93ef62f2-e0a8-4378-b81e-1eb6f1cc7579.png)

## Create AWS Admin User
So as you can see below, I had create differents users and groups, just to learn about it 
![image](https://user-images.githubusercontent.com/93335543/219702109-92bb7736-6852-488b-bf20-7da1b083bc5c.png)

## AWS CLI
I used gitpod to install AWS CLI, but there are some otehr ways, for example you can install aws cli on your local machine. 
Here you have the documentations if you want to do it that way: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
If you want to do it as I do it, you first need to create an account in gitpod (https://www.gitpod.io/), them you have to add an extension on your browser, the name is Gitpod-Always ready to code.

In gitpod you use the following commands to start installing aws-cli
```
$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
$ unzip awscliv2.zip
$ cd aws
$ ./install -i ~/.local/aws-cli -b ~/.local/bin
```
NOTE: you have to be in the workspace 

Then we configure the aws-cli environments:
```
$ export AWS_ACCESS_KEY_ID="[AWS_ACCESS_KEY]"
$ export AWS_SECRET_ACCESS_KEY_ID="[AWS_SECRET_ACCESS_KEY]"
$ export AWS_DEFAULT_REGION="[AWS_REGION]"

or

$ aws configure
AWS Access Key ID [None]: [AWS_ACCESS_KEY]
AWS Secret Access Key [None]: [AWS_SECRET_ACCESS_KEY]
Default region name [None]: [AWS_REGION]
Default output format [None]: json
```
In order to make them persistent we have two options: 
```
$ export gp env AWS_ACCESS_KEY_ID="[AWS_ACCESS_KEY]"
$ export gp env AWS_SECRET_ACCESS_KEY_ID="[AWS_SECRET_ACCESS_KEY]"
$ export gp env AWS_DEFAULT_REGION="[AWS_REGION]"
```

After that we check that everything is working properly:
```
$ aws sts get-caller-identity
{
    "UserId": "[USER_ID]",
    "Account": "[ACCOUNT_ID]",
    "Arn": "arn:aws:iam::[ACCOUNT_ID]:user/[NAME]"
}
```
![image](https://user-images.githubusercontent.com/93335543/219734102-d7377412-bde6-4330-a467-8b643dbd5462.png)

In order to make all this persistent we need to do the following:
In the .gitpod.yml we have to add this:
```
tasks:
  - name: aws-cli
    env: 
      AWS_CLI_AUTO_PROMPT: on-partial
    init: |
      cd /workspace
      curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
      unzip awscliv2.zip
      sudo ./aws/install
      cd $THEIA_WORKSPACE_ROOT
```
Then we need to make the environment variables persistent so we launch the followings commands:
```
$ gp env AWS_ACCESS_KEY_ID="[AWS_ACCESS_KEY]"
$ gp env AWS_SECRET_ACCESS_KEY="[AWS_SECRET_ACCESS_KEY]"
$ gp env AWS_DEFAULT_REGION="[AWS_REGION]"
```
Now if you close the workspace and re-enter you can continue working with yours environmet variables set up. 

## Create a billing alarm and budget
In order to do create a budget with AWS-CLI, I follow this doumentation:
https://docs.aws.amazon.com/cli/latest/reference/budgets/create-budget.html

We are going to create two json files, one call budget.json and the other notifications-with-subscribers.json. You can find the two files in my folder aws/json
In order to do this we need to obtain our AWS Account ID, using the following command in the AWS Console
```
$ aws sts get-caller-identity --query Account --output text
```
Then we launch the followin commands in order to create the budget
```
$ aws budgets create-budget \
  --account-id [ACCOUNT_ID] \
  --budget file://aws/json/budget.json \
  --notifications-with-suscribers file://aws/json/notifications-with-subscribers.json
```

You can also create billing alarms and budgets using AWS
![image](https://user-images.githubusercontent.com/93335543/219740435-9a797651-d4f8-462f-be30-20816bb6e57b.png)

## AWS Well-Architected 
This is a tool that helps you understand the benefits and drawback of decisions you make while building systems on AWS. 
It hae 6 pilars and in each oen you have differents questions according to the theme
1. Operational Excellence
2. Security
3. Reliability
4. Performance
5. Cost Optimization
6. Sustainability 
Here is the link for more information:
https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html
https://docs.aws.amazon.com/wellarchitected/latest/framework/appendix.html

## Conceptual Diagram 
We used LucidChart to create a diagram that describes the funcionality of what we are planning build in ths bootcamp. For this we need to create an account in LucidChart (www.lucidchart.com)
![image](https://user-images.githubusercontent.com/93335543/219760345-5ef07bae-b204-4559-b5d7-6a4ad8eedb97.png)
Here is the link to see my diagram: https://lucid.app/lucidchart/573684be-d1b8-47d2-93a4-7d81ec2a880c/edit?viewport_loc=-233%2C78%2C2888%2C1290%2C0_0&invitationId=inv_d1117d50-2a89-4f64-9fbe-fc831ac12d6f

## Architectural Diagram 
We also used LucidChart to create this diagram 
![image](https://user-images.githubusercontent.com/93335543/219759660-aec1536f-03cc-4cc9-903f-479c0d892ec4.png)
Here is the link to see my diagram: https://lucid.app/lucidchart/9ac22735-4d9f-4b86-b4b5-361743d86b0c/edit?viewport_loc=-469%2C-48%2C3282%2C1465%2C0_0&invitationId=inv_7e5cfc0f-2854-471f-9f35-95debdd004a3





