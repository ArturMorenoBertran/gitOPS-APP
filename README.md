**#############**
**Introduction**
**#############**

This project uses the GitOps methodology for deploying a microservice on AWS, at both code and infrastructure levels, providing a control mechanism in both areas.

This is achieved by automating infrastructure code using Terraform and microservice code through CI/CD pipelines, storing all code in the Git version control system.

Additionally, access to code modification is only permitted through Git, providing control over users making changes and ensuring that the code passes through defined quality filters. 
This prevents administrators or any user from connecting to the AWS infrastructure with an IAM account and making manual changes that could cause issues or simply be difficult to track.

**#############**
**Architecture**
**#############**

As mentioned, this project revolves around GitHub. We have two repositories:

   gitOPS-IAAC: for Terraform code (Infrastructure)
   gitOPS-APP: for application code (Build, Test & Deploy)

Both have a defined workflow that detects changes in the code and applies it to the infrastructure/application.

In this gitOPS-APP repository, you can find the application code, Dockerfile, and Kubernetes definition files are located.
A workflow is defined to obtain the code, build it, test it, and deploy it.
In this project, Maven is used for the build and SonarCloud for code quality validation. 
If all goes well, a Docker image of the code will be built and uploaded to Amazon ECR.
We will use Helm Charts to dynamically create the Docker image name and its tag.
The Kubernetes cluster will detect the tag change in the image and download it from ECR to run the application.

**The application I use, I took it from the course "DevOps Beginners to Advanced with Projects" made by Imran Teli**


**#############**
**Before start**
**#############**

Make sure you have access to your Git repository using some type of credential

This project is based on AWS and its creation will contains charges in your bill.

- Create an IAM user

- Create an access key for your IAM user and store its values in Git Secrets for both repositories.

*Make sure that all services are created in the same region*

- Create S3 bucket. We will use it to store the terraform state.
  Store the bucket name as a secret in Git Secrets only in gitOPS-IAAC repository.

- Create an ECR.
  Store the URL as a secret in Git Secrets only in gitOPS-APP repository.


**#####################**
**CHANGES ON gitOPS-APP REPOSITORY**
**#####################**

- Go to your SonarCloud account, create a new organization, then a new project and generate a token. Add it to Git secrets in gitOPS-APP repository. 
 
- Create three secrets with organization name, project name and URL (https://sonarcloud.io).
 
- On directory "kubernetes/vpro-app" modify the vproingress.yml file : set the tag "host" with your domain name. We will access to our aplication with this URL and then Nginx Ingress will route it.
 
- Replace the enviroment variables in the workflow (.github/workflows/main.yml).

- IT MUST BE READY FOR USE.

