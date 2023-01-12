# Svelts_Contact_form_usnig_Aws_API_Gateway_and_Aws_Lamda

These are the Following Steps for creating Create Dynamic Contact Forms for S3 Static Websites Using AWS Lambda, Amazon API Gateway, and Amazon SES.

We are a preferred AWS consultant and offers the <a href="https://www.ecomstreet.com/aws-consulting-services-company/" target="_blank">best cloud AWS consulting service</a>. Our AWS-certified expert consultants conduct a thorough review and evaluation of your existing IT infrastructure and service interaction model to provide top-notch solutions.

<h3><a href="https://www.ecomstreet.com/aws-consulting-services-company/" target="_blank">Schedule your free consultation today!</a></h3> <br/>

# Architecture Flow
Following is the architecture flow with detailed guidance.

<img width="527" alt="archi" src="https://user-images.githubusercontent.com/122258630/211975574-c57359d5-c117-4806-a1d9-11df29144957.PNG">

In the above diagram, the customer is submitting their inquiry through a “contact us” form, which is hosted in an Amazon S3 bucket as a static website. Information will flow in three simple steps:

1. Your “contact us” form will collect all user information and post to Amazon API Gateway restful service.
2. Amazon API Gateway will pass collected user information to an AWS lambda function.
3. AWS Lambda function will auto generate an e-mail and forward it to your mail server using Amazon SES.

# The Problem:

Having Contact Me like the one below is an integral part of most of the personal and small business websites that are built.

<img width="843" alt="contact_form" src="https://user-images.githubusercontent.com/122258630/211976634-644461e6-8676-49a8-95ee-561fba4e054c.PNG">

I was building one such website, as the entire website is static, I honestly do not want to set up a server just to expose a single endpoint.

# The solution:

I know that cloud functions are something that solves my problem of having an endpoint without actually setting up the server. I chose AWS Lambda as it was much popular. But, the resources and blogs were not enough to give me a step by step guideline.

# What are we building? 

We are going to build a node emailer service that accepts a message in our POST request body and triggers an email to the predefined set of recipients from your Gmail account.
Setup Amazon SES
# Table Of Contents

1. AWS Account Setup.
2. Setting up Lambda.
3. Uploading the emailer code to your lambda.
4. Setup Amazon SES.
5. Creating AWS API Gateway
6. Intigrate API Gateway with lambda.


# 1.AWS Account Setup:

Create an AWS account here. Your account setup will be complete with you entering your Credit card details and verifying your email. Charges are usage-based.

# 2.Setting up Lambda:

1. Naviagte to AWS Console.
2. Choose Lambda under the Find Services input field.
3. You should now be on the lambda functions dashboard which shows the list of your available lambda functions.
4. You should now be on the lambda functions dashboard which shows the list of your available lambda functions.
5. In the next screen, fill in your Function name - emailer and choose Nodejs runtime as we are implementing this using node.

<img width="947" alt="aws_lambda" src="https://user-images.githubusercontent.com/122258630/211990086-5a2727fd-e4ff-4cf1-8467-7b0df89f55c2.PNG">

6. Now you can execute and test your AWS lambda function as directed in the AWS developer guide. Make sure to update the Lambda execution role and follow the steps provided in the Lambda developer guide to create a basic execution role.
7. Add following code under policy to allow Amazon SES access to AWS lambda function:

<img width="946" alt="aws_ses_policy" src="https://user-images.githubusercontent.com/122258630/211991024-55132130-5569-46ff-a03f-af4bed49d1e2.PNG">

8. On Clicking the Create function button you should see Successfully created the function emailer message on the next screen.
9. On scrolling down the page, you will see a sample nodeJS code with index.js.
10. Create a new test with any name of your choice and click on the Test button, you should be getting the response in the Execution Result tab.

# 3.Uploading the emailer code to your lambda:

The Aws lambda IDE for nodeJS does not allow us to install our npm packages on the go. Due to this, we have to get this set up locally in our machine and then upload the code to lambda by zipping it.

1. Download the Zip. It contains the code to be uploaded to your lambda function.
2. If you want to create the zip, the content is present inside this repo where there are a nodemailer dependency and some code to send an email. Make sure to npm install and create a zip from the root directory including your node_modules folder.
3. Once you got the Zip, upload it to the AWS lambda using Actions -> Upload a .zip file option.

<img width="957" alt="lambda_upload" src="https://user-images.githubusercontent.com/122258630/211993503-7c5bda98-c4af-4a26-a60c-1525eee6bd74.PNG">

4. If you open index.js you should be able to see the code where we have given our Email credentials and sending an email.
5. Headers are set to handle CORS errors if you try hitting your lambda from another origin.

# 4.Setup Amazon SES

Amazon SES requires that you verify your identities (the domains or email addresses that you send email from) to confirm that you own them, and to prevent unauthorized use. Follow the steps outlined in the Amazon SES user guide to verify your sender e-mail.

<img width="951" alt="SES_Email" src="https://user-images.githubusercontent.com/122258630/211995965-c472aa27-c413-40cb-a792-1faf5722d209.PNG">

# 5.Creating AWS API Gateway:

1. Create an API to expose this lambda function as a service.
2. Click on Services -> API gateway service from the search bar -> Create API -> REST API -> Build -> API name -> Create.
3. You should be on this screen now.

<img width="948" alt="API" src="https://user-images.githubusercontent.com/122258630/211997975-3cead21e-4388-49e1-adc0-b76614d3c2d3.PNG">

<img width="947" alt="api_created" src="https://user-images.githubusercontent.com/122258630/211997546-593b6c64-376c-4486-9455-36a2525d1039.PNG">

<img width="946" alt="api_resource" src="https://user-images.githubusercontent.com/122258630/211997901-a95f6107-9165-45ae-b63e-1bd5e0195d83.PNG">

4. We need two methods to be created. 1.POST and 2.OPTIONS to handle CORS.

# Creating POST:

1. Actions -> Create Method -> POST -> TICK -> Integration type -> Lambda -> Lambda Function -> emailer -> Save -> OK.

<img width="947" alt="api_lambda" src="https://user-images.githubusercontent.com/122258630/211998506-eecdd95c-5394-4da8-b74a-90cb03d73226.PNG">

2. We need to allow few Headers so that they could be read by the client.
3. Method Response -> Expand the Accordion next to 200.

<img width="946" alt="api_header" src="https://user-images.githubusercontent.com/122258630/211998996-19e297a2-3197-4596-ace2-a3e9f16b2a07.PNG">

4. Add the following headers

<img width="475" alt="header" src="https://user-images.githubusercontent.com/122258630/211999340-8838a3a4-a665-4cb8-82c2-09785f924805.PNG">

5. Go to Integration Response -> Expand Accordion -> Header Mappings -> Make the following.

<img width="473" alt="header_domain" src="https://user-images.githubusercontent.com/122258630/211999526-8126ab0d-db1b-46a3-abba-9429e7cecdd2.PNG">

6. If you have multiple headers being passed from your API, in order to consume them you have to enable it here.
7. You can now do a test from the TEST option -> pass the following in the body.

<img width="475" alt="test" src="https://user-images.githubusercontent.com/122258630/211999730-764df0e0-1ad6-4d13-9a2b-28c6703ac7ff.PNG">

8. Click on Test -> you should get an Email with "HELLO" in the Message.
9. Actions -> Deploy API -> Deployment Stage (New Stage) -> Dev as Stage Name -> Deploy.
10. Your POST API is Now deployed.
11. Copy the INVOKE URL.
12. POST call this INVOKE URL with message param in body to send the email.

# Connecting it all Together

Since we created our AWS Lambda function and provided the API-endpoint access using API gateway, it’s time to connect all the pieces together and test them.

<img width="947" alt="trigger_api" src="https://user-images.githubusercontent.com/122258630/212000793-8282116a-aa82-4cfe-8b11-97d94e29ce11.PNG">

now API_Gateway has been added as a triiger in lambda function.
So now finally go to postman and select post method and enter the api url into postman and in body section add the code which is shown in the following attachements.

<img width="954" alt="postman_api" src="https://user-images.githubusercontent.com/122258630/212001972-bf39d6af-a44e-4a32-a525-d9bbb8098ea1.PNG">

finally i Got the mail in my mail box as shown in the attachement.

<img width="960" alt="mail" src="https://user-images.githubusercontent.com/122258630/212002203-8afc5cc8-1ba3-46ef-9bc3-fa23c4e1d121.PNG">

Now you should be able to submit your contact form and start receiving email notifications when a form is completed and submitted.
