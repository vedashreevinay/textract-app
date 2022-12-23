# textract-application

This project contains source code and supporting files for a serverless application that you can deploy with the SAM CLI. It includes the following files and folders.

- textractapp - Code for the application's Lambda function that calls the textract API.
- src - contains the step functions workflow definition
- events - Invocation events that you can use to invoke the function.
- tests - Unit tests for the application code.
- template.yaml - A template that defines the application's AWS resources.

The application uses several AWS resources, including Lambda functions and Step Functions workflow. These resources are defined in the `template.yaml` file in this project. You can update the template to add AWS resources through the same deployment process that updates your application code.

## Application architecture

![Architecture](/assets/workflow.png)

When the application is deployed, this workflow is setup. The 1st step in the workflow is to call the textrct API to get information from the document. The input to this step is the document name. The workflow calls lambda function which calls the textract API. The output from the API is then written to the output S3 location ( specified when deploying the application) in the 2nd step of the workflow.

## Deploy the sample application

The Serverless Application Model Command Line Interface (SAM CLI) is an extension of the AWS CLI that adds functionality for building and testing Lambda applications. It uses Docker to run your functions in an Amazon Linux environment that matches Lambda. It can also emulate your application's build environment and API.

To use the SAM CLI, you need the following tools.

- SAM CLI - [Install the SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
- [Python 3 installed](https://www.python.org/downloads/)
- Docker - [Install Docker community edition](https://hub.docker.com/search/?type=edition&offering=community)

To build and deploy your application for the first time, run the following in your shell:

```bash
sam build --use-container
sam deploy --guided
```

## Test the application

To test the application, go to Step functions console and locate the workflow created.
Start a new execution and provide the input document you want the app to analyze as shown below. This document should be uploaded to the InputBucket that you specified while deploying the application.

{
"Document": "abc.pdf"
}

After the workflow runs( takes about 30 secs depending on the size of the document), the ouput file should be available in the OutputBucket location that you provided while deploying the application.

## Cleanup

To delete the sample application that you created, use the AWS CLI. Assuming you used your project name for the stack name, you can run the following:

```bash
aws cloudformation delete-stack --stack-name textract-func
```

## Resources

See the [AWS SAM developer guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) for an introduction to SAM specification, the SAM CLI, and serverless application concepts.

Next, you can use AWS Serverless Application Repository to deploy ready to use Apps that go beyond hello world samples and learn how authors developed their applications: [AWS Serverless Application Repository main page](https://aws.amazon.com/serverless/serverlessrepo/)
