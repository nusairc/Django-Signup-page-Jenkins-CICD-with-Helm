#  Setting Up CI/CD for Python Django Application using Helm Charts and Kubernetes -- USE FOR LEARNING/REFERENCE PURPOSE _ DONT CLONE THIS AS IT MAY CAUSE ERROR !

Prerequisites

    Docker is installed and running on your system.
    Minikube is installed and configured for local Kubernetes deployment.
    Jenkins is installed and running on your laptop.
    Helm CLI is installed on your system.

    Step 1: Helm Chart Creation

    Create a new directory named "helm-chart" in your repository root.
    Inside the "helm-chart" directory, create a file named "values.yaml". This file will contain the default configuration values for your Helm Chart.
    Create another file named "deployment.yaml" to define the Kubernetes Deployment resource for your Django application.
    Create a "service.yaml" file to define the Kubernetes Service resource for your Django application.
    Optionally, create an "ingress.yaml" file to define the Kubernetes Ingress resource to expose your application externally.
    Create an "hpa.yaml" file to define the Kubernetes Horizontal Pod Autoscaler resource for your Django application.

    else Open a terminal or command prompt.

    Navigate to the root directory of your project.

    Run the following command to create a Helm Chart for your Python Django application:

      - helm create helm-chart

    Ensure that all the above files are properly configured with the necessary values and specifications for your Django application.


ccess the Jenkins web interface and install the required plugins:
   - GitHub plugin
   - Docker plugin
   - Kubernetes plugin
   - AWS plugin

. Configure your GitHub credentials in Jenkins using the Jenkins Credentials Manager.

. Create two pipeline jobs in Jenkins:
   - Build: Use the "Jenkinsfile" to build the Docker image, push it to ECR, and package the Helm Chart. -[Integration Pipeline]
   - Deploy: Use the "Jenkinsfile-helm" to download the Helm Chart from ECR and deploy it to Minikube.[Deploy Pipeline]

## Helm Chart

The "helm-chart" directory contains the Helm Chart templates for the Django application deployment. Customize the "values.yaml", "deployment.yaml", "service.yaml", and "ingress.yaml" files to match your application's requirements.

## Jenkinsfile

The "Jenkinsfile" in the repository root defines the Jenkins pipeline for the "Build" job. It performs the following steps:
- Git checkout
- Python build and unit test
- Docker build and push
- Update Helm Chart image tag
- Package and push Helm Chart to ECR

## Jenkinsfile-helm

The "Jenkinsfile-helm" in the repository root defines the Jenkins pipeline for the "Deploy" job. It performs the following steps:
- Download Helm Chart from ECR
- Deploy Helm Chart to Minikube

Please update the environment variables in both Jenkinsfiles to match your environment.

## Usage

1. Make necessary changes to your Python Django application and Helm Chart templates.

2. Commit and push the changes to your GitHub repository.

3. Jenkins will automatically trigger the CI/CD pipeline upon each push.

4. Monitor the Jenkins job status to track the build and deployment progress.

## Note

- This is a sample project for demonstration purposes and learning purpose - it may cause errors as it created for academic purpose project.

-  In a production environment, ensure proper security measures and secrets management are in place.

- Make sure to customize the Helm Chart and Kubernetes configurations according to your application's requirements and environment.

- For production deployments, use a secure and private Docker registry and ECR setup.

- In a real-world scenario, consider setting up Jenkins on a dedicated server or using a CI/CD platform, like GitLab CI or GitHub Actions, for better scalability and manageability.

- In jenkins pipeline script update GITHUB,AWS,DOCKERHUB,k8s credentials accordingly. i have used sample variables,it wont work if you used the script without update. also create environment variables in jenkins interface as required. 

Happy coding and deploying!
