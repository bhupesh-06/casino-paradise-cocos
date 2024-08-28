# Setting Up CI/CD Pipeline for Cocos WebGL Project 

## Summary 
The workflow triggers on pull requests to the dev branch.
It builds the project using Node.js, archives the build artifacts, and then deploys these artifacts to the main branch.
It uses GitHub Actions’ predefined actions for checking out code, setting up Node.js, managing artifacts, and performing Git operations.
 
## Workflow Description: Build and Deploy Cocos WebGL Project 
**Workflow Name and Trigger**
- Using  pull_request with Trigger in Dev.

**Trigger**
- The workflow is triggered on a pull request to the dev branch. This ensures that every time a pull request is made or updated for the dev branch, the workflow runs to build and deploy the project.

#Createing Jobs jobs :  Build and Deploy
- Job Name: build-and-deploy. This is an arbitrary name for the job and will be shown in the GitHub Actions UI.
- Runs On: Specifies the type of virtual machine to use for this job. ubuntu-latest means it will run on the latest version of Ubuntu.

## Workflow of Code
- Checkout-Code -  Build Docker Image- Run Build Command in Docker Container- Push Built Site to gh-pages Branch  - Install Dependencies - Build the Project - Archive Build Artifacts - Checkout Main Branch - Download Build Artifacts - Deploy Build Artifacts 

## Step Name: Deploy build artifacts 
**Commands:**
- cp -r build-artifacts/* .: Copies the contents of the build-artifacts directory to the root of the checked-out main branch directory.
- git config --global user.name "github-actions" and git config --global user.email "github-actions@github.com" set up Git configuration for committing changes.
- git add ., git commit -m "Deploy build artifacts from dev branch", and git push origin main commit the changes and push them to the main branch.

## Environment Variable:
- GIT_AUTH_TOKEN: ${{ secrets.GITHUB_TOKEN }}: The GitHub token is used for authentication to push changes to the repository. This token is automatically provided by GitHub Actions and grants the necessary permissions for actions to interact with the repository.
## Push Steps : 
- Configures Git with a user name and email.
- Creates a new branch (gh-pages in this example; you can use any branch name as needed).
- Adds the built site files from the _site directory.
- Commits the changes with a message.
- Pushes the changes to the gh-pages branch on the remote repository.

## Setup Instructions 
 **Setup Instructions for your GitHub Actions workflow**
 
  **Initial Setup**
- Create or Edit the Workflow File:
  
**Navigate to your GitHub repository.**
- Go to the .github/workflows/ directory. If this directory does not exist, create it.
- Create a new file named build-and-deploy.yml (or any other name you prefer) or edit an existing workflow file.

 **Add the Workflow Configuration:**
- Copy and paste the following configuration into your workflow file:

**Configure Secrets:**

- Ensure that you have the GITHUB_TOKEN secret configured in your GitHub repository. This token is automatically provided by GitHub Actions and allows the workflow to authenticate with GitHub for actions like pushing changes.
- Go to your repository on GitHub.
- Navigate to Settings > Secrets and variables > Actions.
- Verify that the GITHUB_TOKEN is available (GitHub creates this by default).
  
# Modifying the Workflow 
 **Modify Node.js Version:**
- Update the node-version field under the Set up Node.js step if your project requires a different version of Node.js.
  
 **Update Build Output Directory:**
- Adjust the path in the Archive build artifacts step to match the directory where your build output is located. 

 **Customize Deployment Logic:** 
- Modify the Deploy build artifacts step if you need different deployment actions. For instance, if you need to deploy to a different branch or use additional deployment scripts, adjust the commands accordingly.

 **Handle Multiple Environments:** 
- If you need to deploy to different environments, you can set up additional jobs or steps in the workflow file. You might use environment-specific branches or deploy different artifacts based on the branch being built.

# Testing Changes
- After modifying the workflow, commit and push your changes to the repository.
- Open a pull request to the dev branch to test the workflow.
- Monitor the Actions tab in your GitHub repository to ensure that the workflow runs as expected and check for any errors or issues.

