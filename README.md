# Deploying a Node.js App to AWS CodeCommit and Setting up an AWS CodePipeline #

**Introduction**
- A Node.js app refers to an application built using the Node.js runtime environment. Node.js is an open-source, server-side JavaScript runtime that allows developers to run JavaScript code outside of a web browser.

- It enables the execution of JavaScript on the server-side, making it possible to build scalable and high-performance web applications.

- Node.js apps can be developed using a variety of frameworks and tools, such as Express.js, Nest.js, Hapi.js, or even by building custom applications without using a specific framework.

- The choice of framework depends on the specific requirements and preferences of the developer or development team.

- Overall, Node.js provides a powerful platform for building server-side applications, enabling developers to leverage their JavaScript skills

- to create scalable, efficient, and high-performance applications for a wide range of use cases.

**Prerequisites:**

- Ensure you have an AWS account.
- Install Git on your local machine Or Using AWS Instance.

  ![aws-instance](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/0cd37442-b6ef-49db-8f48-f9c7678475b8)

 
  `sudo apt update
   sudo apt install git
   git --version`

- Install the AWS Command Line Interface (CLI) on your local machine (Optional; If you want to manage AWS resources through the CLI).

   `sudo apt install awscli`

**Step 1: Set up AWS CodeCommit:**

- Go to the AWS Management Console and search for "CodeCommit".
- Create a new repository in CodeCommit:
- Click on "Create repository".
- Provide a name and optional description for your repository.
- Configure other settings as desired and click "Create".

![creating-repo](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/be7a969f-1721-4d55-9485-5efbcd6ca53e)


![repo-created](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/09f7079c-4ad1-43df-8c70-487b60755ae7)


** Step 2 : GIT CREDENTILAS AND PERMISSIONS/POLICIES**

* Generate git credentials for the IAM user. 
* This access key consists of a user name and a password. 
* These credentials will be used to authenticate your Git commands.

![credentials](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/45967000-1600-46cc-95fa-1269987dbae4)

- When deploying a Node.js app to AWS CodeCommit and setting up an AWS CodePipeline, you need to configure appropriate permissions and policies to ensure that the necessary AWS services can interact with each other. 
- Here are some permissions and policies that you may need to set up:

  * AWS Identity and Access Management (IAM) Roles:The IAM role should have permissions to access the CodeCommit repository,
    invoke CodeBuild projects, and deploy to the desired AWS services (e.g., Elastic Beanstalk, Lambda, EC2).
  
  * CodeCommit Repository Access: Ensure that the IAM user or role used for interacting with CodeCommit
    has permissions for actions like `codecommit:GitPull`, `codecommit:GetRepository`, and `codecommit:UploadArchive`.

  * CodeBuild Project Permissions: The IAM role should have permissions to read from the CodeCommit repository,
    access build artifacts, and perform any additional actions required.

  * Deployment Permissions: Grant the IAM role used by the pipeline appropriate permissions to deploy to the desired AWS service.

  * Bucket Permissions: If your deployment process involves storing artifacts or other files in an Amazon S3 bucket, 
    ensure that the appropriate IAM permissions are set for the bucket.

    ![attach peolicy](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/d996f96b-9a07-4cc0-b691-0c5c71dcc3e9)


    ![policy](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/5ee6b964-bec7-4e32-b711-ee29e598966d)


**Step 3: Do Some Back-end Configuration, Set up Git and Push to CodeCommit:**

*Update and upgrade Ubuntu: sudo apt update sudo apt upgrade*

*To get the loation of the node.js software from Ubuntu repositories, run: curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -*

*Install Node.JS on the server: `sudo apt-get install nodejs -y`*

* Initialise your project with the `npm init` command

The above command installs both nodejs and NPM. NPM is a package manager for Node and it is used to install Node modules & packages such as the package-lock.json and package.json and to manage dependency conflicts.
Verify the Node and NPM installation:
`
node -v
npm -v
`
- Initialize a Git repository on your local machine:
- Open a terminal and navigate to your project directory.
- Run the command: `git init`.
- Add your Node.js app files to the Git repository:
- Run the command: `git add .` (including the dot) to add all files to the repository.
- Alternatively, you can specify individual files: `git add <filename>.`
- Commit the files to the repository:
- Run the command: `git commit -m "Initial commit"` (replace the commit message as needed).
- Set up the remote repository:
- Retrieve the HTTPS clone URL from the CodeCommit repository page.
- Run the command: `git remote add origin <clone_url> (replace <clone_url> with the actual URL)`.
- Push the code to CodeCommit and create your pipeline:

  ![pipe1](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/eb644fb6-e7eb-44e7-8b61-45892dc78d04)
  
 ![pipe2](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/ec361722-80ac-406b-8f63-33fe5b92d6d2)

![pipe3 4](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/6b51f5e4-3816-4a8f-82a1-40ecc089797d)

![codebuild1](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/7c8fde76-e8b0-4979-a5cb-09e39ca3c075)

![codebuild2](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/e773998c-000d-4f06-901c-a641d2dceb97)

- Ensure your buildspec.yml file (defines the build and deployment instructions for CodeBuild, specifying how to build, test, and deploy your application during the build process.) is at the root directory 

![buildspec yml](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/8fe49b5d-9c31-4e96-aedf-b5fa6b6e9b0e)


- Run the command: git push origin master (pushing to the master branch by default).
  
- Provide your AWS CodeCommit credentials if prompted.

  ![attach peolicy](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/f95a26b6-c167-49d9-a41b-639cc970d3fc)

 ![policy](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/cec1fe8d-54c6-4795-a16f-dfaca862f944)

- Create an ECR Repository: In the AWS Management Console, navigate to the ECR service and create a new repository.

![repo-created](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/6a48f7a6-ea5b-4e0f-9a70-c1d3d806da1b)


- Build and Tag Docker Images: In your project's build process such as in the `buildspec.yml`

  `aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 936417334805.dkr.ecr.us-east-1.amazonaws.com`

  `docker build -t my-nodejs-app`  `docker tag my-nodejs-app:latest 936417334805.dkr.ecr.us-east-1.amazonaws.com/my-nodejs-app:latest`
    
![red permission ](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/a2e508c7-f521-470b-a82b-0f095c4a67ff)


![buil-success](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/99c4aebc-3eae-44aa-ba92-d62467f932ec)

![finished](https://github.com/bakare-rasheed/pipeline-project/assets/114327344/c70ca9c1-ed31-4f9f-9aed-a659cab69a6c)





  
- 
