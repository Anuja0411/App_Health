# App Health Check API - with CodePipeline
This repo is created to accomodate the code and documentation required to setup a AWS environment that provides Cloudformation templates to setup a sample API using Lambda and API Gateway to provide a JSON response containing details of the App and the Git Hash.

### Services used
- API Gateway
- Lambda
- CodePipeline
- Cloudformation

### Deployment Steps
<br>

> ### Pre-requisites<br>
>For the purposes of CodePipeline a codestar connection needs to be created manually with GitHub. Steps to create the connection are available here. <br>https://docs.aws.amazon.com/codepipeline/latest/userguide/connections-github.html<br>
<b>Please take a note of the connection ARN. This will be used as a input to the CFN template that deploys the CodePipeline</b><br>

### Deploying CodePipeline<br>
Code for deploying CodePipeline is available in the `codepipeline.yaml` file. This template creates the complete pipeline to automate release of the API as changes are made to the repo. ( Repo details will be shared separately via email ). Using the repo details create the GitHub(2) connection in CodePipeline and note the arn.
Name of the Cloudformation stack `API_HEALTH_CICD`.
<br> Parameters as follows:
- CodePipelineBucketName: codepipeline-ap-southeast-2-xxxxxxxxxx
- CodeStarConnectionArn: arn:aws:codestar-connections:ap-southeast-2:xxxxxxxx:connection/8d554689-4597-4973-ba04-592a23a01d8f
- Environment:	Dev
- GitHubBranch:	main
- GitHubRepository:	owner/repo
<br>
Deploying the pipeline and triggering it, will automatically create the new CFN stack for the API Gateway and Lambda. The new stack created will be by the name `API-HEALTH-CHECK`. 

<img src="./images/pipeline.png">

Once the `API-HEALTH-CHECK` stack is launched completely, move to the `Outputs tab` of the stack's console. You will see the API gateway URL against the key, `APIGatewayURL`.

<img src="./images/apihealthcheck.png">

 Clik on the link and you should receive a response similar to the following details,<br>  `{"git_hash": "0e911cd3b58f46572683ec2f34d2cbea397f1c1d", "app_version": "1.0", "app_name": "API-HEALTH-CHECK"}` 
<br><br>We have now setup the health API succesfully.<br>

<img src="./images/response.png">
<br>

### Deploying the API Gateway ( manual launch not required )
Code for deploying the API gateway is available in the `cfn.yaml` file. This template creates an API gateway backed by a Lambda function. Name of the Cloudformation stack `API-HEALTH-CHECK`.<br>
Parameters as follows:
- AppName:	API-HEALTH-CHECK
- AppVersion:	1.0
- GitHash:	xxxxxx <br>

This stack is launched automatically as part of the pipeline.

>> Any changes made to the branch `main` will trigger a deployment and the corresponding Git commit hash can be seen in the reponse of the API. Details regarding access to the Repo will be sent via email separately.