## AWS CodeStar CDK
A package used to deploy a lambda function into AWS CodeStar via CDK.

###Description
This package creates a lambda function, accessible through an API Gateway, editable via commits to AWS CodeCommit and fully monitored using AWS CodeStar. 
The function uses gradual code deployment Linear10PercentEvery3Minutes, which means any commits will gradually be deployed and 10 percent of the load will be sent to the new deployment every 3 minutes. If you want to change that, edit the line in 

```aws_codestar_cdk/files/source.zip/template.yml```

More info:
https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/automating-updates-to-serverless-apps.html

The function's runtime is specified to python3.6 and the lambda handler (initially called code within your function) is specified to manage.runner (manage is the file name, a.k.a manage.py and runner is the function name within the file).
All that can also be changed by editing the template.yml file.

Deploying this package creates 4 CloudFormation stacks in total.

The first one uploads the files toolchain.yml and source.zip into AWS S3, in order to be accessed by CodeStar.
The second stack is the CDK stack, that creates the CodeStar project.
The third stack is the CodeStar stack, which specifies, what the Project should create. It's contents can be edited by editing

```aws_codestar_cdk/files/toolchain.yml``` 

The fourth stack is the lambda function stack, which can be edited by editing the template.yml file.

###Prerequisites
In order to operate the package, you must first install it, using
 
```pip install aws-codestar-cdk```

You also need to have an AWS account with a confugured AWS CLI. Here's how to do it:

https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html

### How to use
Install the package by pip using 
```pip install aws-codestar-cdk```
Afterwards, import the main file into your project and call the Main classes constructor.
The arguments for the constructor are the scope, your project name, list of subnet IDs where the function should be deployed and list of security group IDs for the function.
It should look something like this:
```from aws_cdk import core
from aws_codestar_cdk.main import Main

app = core.App()

main = Main(app, 'project-name', ['subnet-1', 'subnet-2'], ['sg-1', 'sg-2'])

app.synth()
```
