# INTRODUCTION

In this repository I use bash scripts to send messages to SQS and escalete EC2 instances depending on how saturated is the queue. For that purpose, 3 processes are defined: 

* Send 100 messages to SQS (defined in AWS_Script script)
* Consum ("Delete") messages from the queue every 6 seconds (delte_messages script)
* Create or Delete EC2 instances depending on the STATUS (manage_instances script) 

For the STATUS I have defined an strategy: 

STATUS = (((n messages in the queue + 9)/10) - N active instances) 

* If the STATUS < 0 --> Delete EC2 instance
* If the STATUS > 0 & N active instances <= 5 --> Create EC2 instance

Considerations: 
* I set a limit of 5 maximum number of instances active at the same time
* The instances cretated are t2.micro in order to don't inccur in costs

# REQUIREMENTS

* Install AWS CLI: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html
* Create a user with EC2 and SQS full access permisions from the IAM: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html
* Configure the user created with the tokens generated in the AWS CLI: https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html
* Configure your URL in each script at the top of the script

# WORKFLOW 

To run the process, you only need to run the AWS_Script from the terminal with the command (make sure you are on the folder where the script is located): 

bash AWS_Script

This script will send the 100 messages and then will call the other two scripts. Note thas these other 2 scripts must be launched on parallel. 

