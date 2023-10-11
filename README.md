## Instructions

<ol>
<li>Clone the source code repository "aws-codedeploy-github-actions-deployment".</li>

    git clone https://github.com/aws-samples/aws-codedeploy-github-actions-deployment.git

<li>Create an empty repository in your personal GitHub account. Clone the repo to your computer and ignore the warning about cloning an empty repository.</li>

    git clone https://github.com/<username>/<repoName>.git
    
<li>Copy the code from the "aws-codedeploy-github-actions-deployment" repo to the newly created repo.</li>

    cp -r aws-codedeploy-github-actions-deployment/. <new repository>

<li>Use the following commands to point to your GitHub repo:</li>

    git remote remove origin
	git remote add origin <your repository url>
	git branch -M main
	git push -u origin main

 <li>Deploy CloudFormation template by first opening AWS CloudFormation console.</li>
 
 <li>Make sure you're using region <i>us-east-1</i></li>

 <li>If this is a new account, click <i>Create New Stack</i>. Otherwise click <i>Create Stack</i>.</li>

 <li>Click <i>Template is Ready</i>.</li>

 <li>Click <i>Upload a template file</i>.</li>

 <li>Click <i>Choose file</i> and navigate to the <i>template.yml</i> file in your local repo at <i>&lt;repoName&gt;/cloudformation/template.yaml</i></li>

 <li>Select the <i>template.yml</i> file and click <i>Next</i>.</li>

 <li>Enter <i>CodeDeployStack</i> as the stack name.</li>

 <li>Enter <i>&lt;username&gt;/&lt;reponame&gt;</i> as GitHubRepoName and click <i>Next</i>.</li>

 <li>Keep the stack options and click <i>Next</i>.</li>

 <li>Select the acknowledgement box to allow for the creation of IAM resources and click <i>Submit</i>.</li>

 <li>On the AWS CloudFormation console, select the <i>Outputs</i> tab and take note of the Amazon S3 bucket name, labeled <i>DeploymentBucket</i>, and the ARN of the GitHub IAM Role, labeled <i>GithubIAMRoleArn</i>.</li>

 <li>Navigate to the <i>deploy.yml</i> workflow file at <i>/.github/workflows/deploy.yml</i> from your project root directory.</li>

 <li>Replace <i>##s3-bucket##</i> with the name of the S3 bucket.</li>

 <li>Replace <i>##region##</i> with <i>us-east-1</i>.</li>

 <li>Navigate to the <i>after-install.sh</i> file at <i>aws/scripts/after-install.sh</i></li>

 <li>Add, commit, and push the changes:</li>

    git add .
	git commit -m “Initial commit”
	git push

 <li>Set up GitHub secrets by first navigating to your github repository and selecting the Settings tab.</li>

 <li>Select <i>Secrets and variables</i> on the left menu bar.</li>

 <li>Select <i>Actions</i>, then select <i>New repository secret</i>.</li>

 <li>Enter the secret name as <i>IAMROLE_GITHUB</i>.</li>

 <li>Enter the value as the ARN of the GitHub IAM Role that was copied from the CloudFormation output section.</li>

 <li>Integrate the CodeDeploy Application with GitHub by first navigating to the CodeDeploy console.</li>

 <li>In the navigation pane on the left, expand Deploy, then choose Applications.</li>

 <li>Click the application <i>CodeDeployAppNameWithASG</i> to view details.</li>

 <li>Click the <i>Deployments</i> tab and click <i>Create deployment</i>.</li>

 <li>Select Deployment group <i>CodeDeployGroupName</i>.</li>

 <li>For Revision type, select <i>My application is stored in GitHub</i>.</li>

 <li>Type in a name for the GitHub token name and click <i>Connect to GitHub</i>.</li>

 <li>The web page prompts you to authorize CodeDeploy to interact with GitHub for your application. Authorize the application.</li>

 <li>After you've authorized the application, click <i>Cancel</i> and don't create deployment.</li>

 <li>Go to your GitHub Repo and select the <i>Actions</i> tab.</li>

 <li>Click <i>Build and Deploy</i> on the left side.</li>

 <li>Click <i>Run workflow</i> dropdown, then click <i>Run workflow</i>.</li>

 <li>After a few seconds, the workflow will be displayed. When it's displayed, select <i>Build and Deploy</i>.</li>

 <li>You'll see two stages, <i>Build and Package</i> and <i>Deploy</i>.</li>

 <li>The <i>Build and Package</i> stage builds the SpringBoot application, generates the war file, then uploads it to the S3 bucket.</li>

 <li>After this stage, you should be able to the war file in the S3 bucket.</li>

 <li>The <i>Deploy</i> stage triggers the deployment.</li>

 <li>Navigate to the CodeDeploy console and select the application name.</li>

 <li>You will see the status as <i>Succeeded</i> if the deployment is successful.</li>

 <li>Navigate to the CloudFormation console, select the created stack, <i>CodeDeployStack</i>, and select the <i>Outputs</i> section.</li>

 <li>Click the URL next to <i>WebappUrl</i>.</li>

 <li>You should see the message, <i>Congratulations. You have successfully deployed the sample Spring Boot Application.</i></li>

 <li>The deployment can be automated by editing your <i>deploy.yml</i> file.</li>

<li>Change:</li>
    
    workflow_dispatch: {}
to:
	
    #workflow_dispatch: {}
  	push:
	    branches: [ main ]
	pull_request:
</ol>


## Clean up

    1. Empty the Amazon S3 bucket:
    2. Delete the CloudFormation stack (CodeDeployStack) from the AWS console.
    3. Delete the GitHub Secret (‘IAMROLE_GITHUB’)
        a. Go to the repository settings on GitHub Page.
        b. Select Secrets under Actions.
        c. Select IAMROLE_GITHUB, and delete it.

