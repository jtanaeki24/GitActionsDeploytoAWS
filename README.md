## Instructions

1. Clone the source code repository "aws-codedeploy-github-actions-deployment".
	git clone https://github.com/aws-samples/aws-codedeploy-github-actions-deployment.git
2. Create an empty repository in your personal GitHub account. Clone the repo to your computer and ignore the warning about cloning an empty repository.
	git clone https://github.com/<username>/<repoName>.git
3. Copy the code from the "aws-codedeploy-github-actions-deployment" repo to the newly created repo.
	cp -r aws-codedeploy-github-actions-deployment/. <new repository>
4. Use the following commands to point to your GitHub repo:
	git remote remove origin
	git remote add origin <your repository url>
	git branch -M main
	git push -u origin main
5. Deploy CloudFormation template by first opening AWS CloudFormation console.
6. Make sure you're using region us-east-1
7. If this is a new account, click "Create New Stack". Otherwise click "Create Stack".
8. Click "Template is Ready"
9. Click "Upload a template file"
10. Click "Choose file" and navigate to the "template.yml" file in your local repo at "<repoName>/cloudformation/template.yaml”.
11. Select the "template.yml" file and click Next.
12. Enter "CodeDeployStack" as the stack name.
13. Enter <username>/<reponame> as GitHubRepoName and click Next.
14. Keep the stack options and click Next.
15. Select the acknowledgement box to allow for the creation of IAM resources and click Submit.
16. On the AWS CloudFormation console, select the Outputs tab and take note of the Amazon S3 bucket name, labeled "DeploymentBucket", and the ARN of the GitHub IAM Role, labeled "GithubIAMRoleArn".
17. Navigate to the "deploy.yml" workflow file at "/.github/workflows/deploy.yml" from your project root directory.
18. Replace "##s3-bucket##" with the name of the S3 bucket.
19. Replace "##region##" with "us-east-1"
20. Navigate to the "after-install.sh" file at "aws/scripts/after-install.sh"
21. Add, commit, and push the changes:
	git add .
	git commit -m “Initial commit”
	git push
22. Set up GitHub secrets by first navigating to your github repository and selecting the Settings tab.
23. Select "Secrets and variables" on the left menu bar.
24. Select "Actions".
25. Select "New repository secret".
26. Enter the secret name as "IAMROLE_GITHUB".
27. Enter the value as the ARN of the GitHub IAM Role that was copied from the CloudFormation output section.
28. Integrate the CodeDeploy Application with GitHub by first navigating to the CodeDeploy console.
29. In the navigation pane on the left, expand Deploy, then choose Applications.
30. Click the application "CodeDeployAppNameWithASG" to view details.
31. Click the "Deployments" tab and click "Create deployment".
32. Select Deployment group "CodeDeployGroupName".
33. For Revision type, select "My application is stored in GitHub".
34. Type in a name for the GitHub token name and click "Connect to GitHub".
35. The web page prompts you to authorize CodeDeploy to interact with GitHub for your application. Authorize the application.
36. After you authorized the application, click Cancel and don't create deployment.
37. Go to your GitHub Repo and select the Actions tab.
38. Click "Build and Deploy" on the left side.
39. Click "Run workflow" dropdown, then click "Run workflow".
40. After a few seconds, the workflow will be displayed. When it's displayed, select Build and Deploy.
41. You'll see two stages, "Build and Package" and "Deploy".
42. The "Build and Package" stage builds the SpringBoot application, generates the war file, then uploads it to the S3 bucket.
43. After this stage, you should be able to the war file in the S3 bucket.
44. The "Deploy" stage triggers the deployment.
45. Navigate to the CodeDeploy console and select the application name.
46. You will see the status as "Succeeded" if the deployment is successful.
47. Navigate to the CloudFormation console, select the created stack, "CodeDeployStack", and select the "Outputs" section.
48. Click the URL next to "WebappUrl".
49. You should see the message, "Congratulations. You have successfully deployed the sample Spring Boot Application."
50. The deployment can be automated by editing your "deploy.yml" file.
51. Change:
	workflow_dispatch: {}
    to:
	#workflow_dispatch: {}
  	push:
	    branches: [ main ]
	pull_request:

