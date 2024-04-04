We host an interactive webpage that receives Lambda-generated actions and then render the game playing in real time

We package our code and dependencies as a Docker container image. Then upload the image to our container registry hosted on Amazon Elastic Container Registry (Amazon ECR). When invoking the function, Lambda deploys the container image to an execution environment.

Here is the documentation:
https://docs.aws.amazon.com/lambda/latest/dg/lambda-deploy-functions.html#deploying-containers
https://docs.aws.amazon.com/lambda/latest/dg/python-image.html#python-image-instructions

Make sure that you have the latest version of the AWS CLI and Docker installed. 
Copy and paste providede key and id into ~/.aws/credentials.

Step 1: Prepare Your Docker Image locally
To build and deploy your application, run the following in your shell:
```bash
docker build --platform linux/arm64 -t docker-image:test .
```
```bash
docker run --platform linux/arm64 -p 9000:8080 docker-image:test  
```

Test: 
```bash
curl "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{}'
```

Step 2: Create an ECR Repository and push the Docker Image to ECR
Open the Amazon ECR Console: Go to the Amazon ECR console in your AWS account.
Create Repository: Click "Create repository" and follow the instructions by clicking the "View push commands" buttom.

Step 3: Create a Lambda Function with the Container Image
Open the Lambda Console: Go to the AWS Lambda console and click on "Create function."
Choose "Container image": Select the "Container image" option for your Lambda function's source.
Configure the Function: Fill in the function name and choose the container image you uploaded to ECR. You'll need to select the image URI by clicking on "Browse images," choosing your repository, and then selecting the image.
Define Execution Role: Ensure the Lambda function has an execution role with appropriate permissions. This role needs at least basic execution permissions for Lambda, and any additional permissions your function requires to interact with AWS services.

Create Function: Click "Create function" to create your Lambda function.

Step 4: Test Your Lambda Function