 


Contents
1.	Abstract	3
2.	Introduction	3
3.	Containerizing and Deploying TempConverter with Podman and Docker Hub	3
3.1.	Install Podman for Windows	3
3.2.	Install MySQL	4
3.3.	Log in to Docker Hub	4
3.4.	Create Container Image for TempConverter Application	5
3.5.	Push the Image to Docker Hub	7
3.6.	Update HTML Title and Create Dev Tag	7
3.7.	Deploy the Application Locally with Podman	8
3.8.	Access the Application	9
4.	Deploying TempConverter with Azure Container Apps and Azure CLI	10
4.1.	Why Chose Azure Container Apps	10
4.2.	Install Azure CLI	11
4.3.	Log In to Azure:	11
4.4.	Create Deployment Template (.json)	12
4.5.	Deployment of the Application Using the Template	14
4.6.	Accessing the Application	15
5.	Deploying TempConverter with OpenShift	15
5.1.	Setting Up OpenShift Environment	15
5.2.	Creating the Deployment Configuration	15
5.3.	Deploying the Resources	18
5.4.	Accessing the Application	19
6.	Conclusion	19
7.	References	20


 
	Abstract

This project focuses on containerizing and deploying the TempConverter application, a web-based tool for converting temperatures from Celsius to Fahrenheit. The application, originally not containerized, was transformed to operate within a containerized environment using modern DevOps practices. The process included setting up a MySQL database, containerizing the application with Podman, and deploying it on Docker Hub. Subsequently, the application was deployed on two different container orchestration platforms: Azure Container Apps and OpenShift. Each platform was chosen for its unique features; Azure for its serverless and scalable environment, and OpenShift for its comprehensive management of Kubernetes clusters. Detailed documentation and validation of each step ensure reproducibility and clarity, providing a robust guide for similar projects. This endeavor not only demonstrates the practical application of containerization and orchestration but also highlights the scalability and reliability improvements achieved through these technologies.

	Introduction

This project demonstrates the process of containerizing and deploying the TempConverter application, which converts temperatures from Celsius to Fahrenheit. The project highlights the use of modern DevOps tools and practices to manage the application across different container orchestration platforms.

Problem Definition
The challenge is to modernize the TempConverter application's deployment process by containerizing it and configuring it to connect to a MySQL database. The application needs to be deployed on two different container orchestration platforms to showcase their capabilities.

Existing Solutions
Traditional deployment methods involve extensive configuration and maintenance. Containerization technologies like Docker and Podman package applications with their dependencies, ensuring consistency across environments. Orchestration platforms like Kubernetes, OpenShift, and Azure Container Apps manage containers at scale, providing features like automatic scaling and self-healing.

Approach and Methodology
	Containerization with Podman: Created a Dockerfile to define the application's environment, installed dependencies, exposed ports, and specified the command to run the application.
	Database Integration: Set up and configured a MySQL database to work with the containerized application, using environment variables for secure connection details.
	Deployment on Docker Hub: Pushed the container image to Docker Hub, making it accessible for deployment.
	Deployment on Azure Container Apps: Used a YAML template to deploy the application in a scalable manner, including load balancing and scaling policies.
	Deployment on OpenShift: Created OpenShift templates for proper setup and management of the application and database.
	Validation and Documentation: Documented each step with command validations, screenshots, and results to provide a clear workflow.

Results and Technical Solutions
The project successfully containerized and deployed the TempConverter application across different platforms, enhancing its scalability and reliability. Detailed documentation provides insights into best practices and methodologies for similar projects, serving as a practical guide for understanding containerization and orchestration concepts in the DevOps domain.

	Containerizing and Deploying TempConverter with Podman and Docker Hub

	Install Podman for Windows

To begin the containerization process, Podman was installed on a Windows 10 machine. Podman is a daemon-less container engine that allows for the creation and management of containers.

	Download and Installation:
	The Podman installer was downloaded from the Podman GitHub releases page.
	The installer was executed, and the installation prompts were followed.


Validation Command:

podman --version

This command displays the installed version of Podman, confirming its successful installation.

 
Image 1 Command prompt screenshot showing the Podman version.

	Install MySQL

Next, MySQL was installed to provide the necessary database services for the TempConverter application.

	Download and Installation:
	The MySQL installer was downloaded from the MySQL official website.
	The installer was executed, and the installation steps were followed.

Validation Command:

mysql --version

This command displays the installed version of MySQL, confirming its successful installation.

 
Image 2 Command prompt screenshot showing the MySQL version.

	Log in to Docker Hub

A Docker Hub account is essential for storing and managing container images.

	Account Creation and Repository Setup:
	A Docker Hub account was created by visiting Docker Hub and following the sign-up process.
	A new repository named tempconverter was created within the Docker Hub account.

Validation Command:

podman login --gusic.pavao@gmail.ocm --******************** docker.io
This command logs into Docker Hub using Podman.

 
Image 3 Command prompt screenshot showing sucessful login to DockerHub

 
Image 4 Screenshot of the Docker Hub displaying the newly created tempconverter repository.

	Create Container Image for TempConverter Application

The TempConverter application was containerized by creating a Dockerfile and building the image using Podman.

	Clone the Repository:

git clone https://github.com/jstanesic/tempconverter.git
cd tempconverter
	Create Dockerfile:

FROM python:3.9
WORKDIR /app
COPY ./app
RUN pip install --no-cache-dir -r requirements.txt
EXPOSE 5000
ENV NAME World
CMD ["python","app.py"]

Explanation of Dockerfile Contents:
	FROM python:3.9: Specifies the base image, which is Python 3.9. This ensures that the container has Python installed.
	WORKDIR /app: Sets the working directory to /app inside the container.
	COPY . /app: Copies the current directory contents into the container at /app.
	RUN pip install --no-cache-dir -r requirements.txt: Installs the necessary Python packages listed in requirements.txt.
	EXPOSE 5000: Exposes port 5000, which is the port that the Flask application runs on.
	ENV NAME World: Sets an environment variable NAME with a default value of World.
	CMD ["python", "app.py"]: Specifies the command to run the application.

 
Image 5 Screenshot of the project files containing Dockerfile

	Build the Docker Image:

podman build -t tempconverter:latest .

Validation Command:

podman images

This command lists all the container images available locally, confirming the creation of the TempConverter image.
 

 
Image 6 Command prompt screenshot showing the list of container images, highlighting the TempConverter

	Push the Image to Docker Hub

The container image was then pushed to Docker Hub for wider accessibility.

	Tag the Image:

podman tag tempconverter:latest docker.io/pavaogusic/tempconverter:latest

This command tags the tempconverter:latest image with the repository name on Docker Hub. Tagging is important as it specifies the target repository and tag where the image will be stored on Docker Hub.

	Push the Image:

podman push docker.io/pavaogusic/tempconverter:latest

This command pushes the tagged image to Docker Hub. Pushing the image uploads it to the specified repository, making it available for use in different environments.

 
Image 7 Screenshot of the Docker Hub page showing the tempconverter:latest image in the repository.

	Update HTML Title and Create Dev Tag

The HTML title was updated to reflect the new name, and a new image tag was created and pushed

	Edit index.html File:

<title>TempConverter</title>

This step updates the title tag in the HTML file to "TempConverter," ensuring the web application displays the correct title.

	Build the Docker Image with Updated Title:

podman build -t tempconverter:dev .

This command rebuilds the Docker image with the updated HTML title and tags it as tempconverter:dev.

	Tag and Push the New Image:

podman tag tempconverter:dev docker.io/pavaogusic/tempconverter:dev
podman push docker.io/pavaogusic/tempconverter:dev

These commands tag the newly built image with the dev tag and push it to Docker Hub.

 
Image 8 Screenshot of the Docker Hub page showing the tempconverter:dev image in the repository.

	Deploy the Application Locally with Podman

The application and its database were deployed locally using Podman.

	Create a Network:

podman network create tempconverter-network 

This command creates a custom network named tempconverter-network. Using a custom network ensures that the MySQL and TempConverter containers can communicate with each other.

Validation Command:

podman network ls

This command lists all available networks, confirming that the tempconverter-network has been created.





	Start MySQL Container:

podman run --name mysql --network tempconverter-network -e MYSQL_ROOT_PASSWORD=rootpassword -e MYSQL_DATABASE=tempconverter -e MYSQL_USER=student -e MYSQL_PASSWORD=studentpassword -d mysql:8

This command starts a MySQL container with the necessary environment variables for database configuration on the created network.

	Start TempConverter Application Container:

podman run --name tempconverter --network tempconverter-network -e DB_USER=student -e DB_PASS=studentpassword -e DB_HOST=mysql -e DB_NAME=tempconverter -p 5000:5000 -d docker.io/pavaogusic/tempconverter:latest

This command starts the TempConverter application container, connecting it to the MySQL container via the created network and exposing port 5000.

Validation Command:

podman ps

This command lists all running containers, confirming the operation of both the MySQL and TempConverter containers.

 
Image 9 Command prompt screenshot showing the list of running containers, highlighting the MySQL and TempConverter containers.


	Access the Application

After completing all the steps to deploy the TempConverter application locally using Podman, the application can be accessed via a web browser. Opening a web browser and navigating to http://localhost:5000 should display the TempConverter application interface, featuring a form for temperature conversion from Celsius to Fahrenheit. The interface will also include the user's name and college as configured in the environment variables. This confirms that the application is running correctly and is connected to the MySQL database.


 
Image 10 Screenshot of the web browser displaying the TempConverter application interface with your name and college, showing that the application is functioning as expected.

Setting environment variables:

Before running Flask application, set the environment variables in Windows CMD. These environment variables ensure that the Flask application running in the container can connect to the MySQL database.

set DB_USER=student
set DB_PASS=studentpassword
set DB_HOST=mysql
set DB_NAME=tempconverter
set STUDENT=YourName
set COLLEGE=YourCollege

Running the Flask application within the container:

After setting these environment variables, you can run your Flask application within the container using the podman run command as shown in step 3.7. The Flask application will use these environment variables to connect to the MySQL database running in the container.

By following these steps, the TempConverter application was successfully containerized, deployed, and managed using Podman and Docker Hub, providing a clear and reproducible workflow for understanding these processes.

	Deploying TempConverter with Azure Container Apps and Azure CLI

	Why Chose Azure Container Apps

Azure Container Apps is a serverless container service that enables developers to build and deploy applications at scale. The reasons for choosing Azure Container Apps include:
	Serverless Model: It abstracts the underlying infrastructure, allowing developers to focus on application code.
	Scalability: Automatically scales out to meet demand, ensuring efficient resource utilization.
	Integration with Azure Ecosystem: Seamlessly integrates with other Azure services like Azure Functions, Azure Logic Apps, and Azure Cognitive Services.
	Built-in Observability: Provides built-in monitoring, logging, and tracing capabilities.
	Security: Ensures a secure environment with compliance certifications and security features like network isolation and managed identities.
 
	Install Azure CLI

Azure CLI is a command-line tool that provides a set of commands used to manage Azure resources.

	Download and Install Azure CLI:
	Visit the official Azure CLI installation page and download the installer for Windows.
	Follow the installation instructions provided on the page.

	Validation Command:

az --version

This command displays the installed version of Azure CLI, confirming its successful installation. It ensures Azure CLI is correctly installed and ready to use

 
Image 11 Screensho of he command prompt displaying the Azure CLI version.

	Log In to Azure:

az login

A web browser window will open, prompting for Azure credentials to authenticate the Azure CLI session, enabling the management of Azure resources. Upon successful login through the web browser, the console will display all related available subscriptions. The user will need to select one by entering the number displayed next to the subscription. This selection is essential to ensure that the commands executed are applied to the correct Azure subscription, thereby preventing any unintended changes or actions on the wrong subscription.

 
Image 12 Screenshot of the successful login and selected subscription plan.


	Create Deployment Template (.json)

A JSON template is required to deploy the TempConverter application and MySQL database to Azure Container Apps. Below is the JSON template with detailed explanations of each section.

{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "type": "Microsoft.App/managedEnvironments",
      "apiVersion": "2023-08-01-preview",
      "name": "tempconverter-env",
      "location": "eastus",
      "properties": {}
    },
    {
      "type": "Microsoft.DBforMySQL/flexibleServers",
      "apiVersion": "2021-05-01",
      "name": "tempconverter-mysql",
      "location": "eastus",
      "properties": {
        "administratorLogin": "student",
        "administratorLoginPassword": "studentpassword",
        "version": "5.7",
        "storage": {
          "storageSizeGB": 20
        },
        "createMode": "Default",
        "sku": {
          "name": "Standard_B1ms",
          "tier": "Burstable",
          "family": "Gen5",
          "capacity": 1
        }
      }
    },
    {
      "type": "Microsoft.App/containerApps",
      "apiVersion": "2023-08-01-preview",
      "name": "tempconverter-app",
      "location": "eastus",
      "properties": {
        "managedEnvironmentId": "[resourceId('Microsoft.App/managedEnvironments','tempconverter-env')]",
        "configuration": {
          "ingress": {
            "external": true,
            "targetPort": 5000,
            "traffic": [
              {
                "latestRevision": true,
                "weight": 100
              }
            ]
          },
          "activeRevisionsMode": "Multiple",
          "dapr": {
            "enabled": false
          }
        },
        "template": {
          "containers": [
            {
              "image": "docker.io/pavaogusic/tempconverter:latest",
              "name": "tempconverter",
              "env": [
                {
                  "name": "DB_USER",
                  "value": "student"
                },
                {
                  "name": "DB_PASS",
                  "value": "studentpassword"
                },
                {
                  "name": "DB_HOST",
                  "value": "tempconverter-mysql.mysql.database.azure.com"
                },
                {
                  "name": "DB_NAME",
                  "value": "tempconverter"
                },
                {
                  "name": "STUDENT",
                  "value": "YourName"
                },
                {
                  "name": "COLLEGE",
                  "value": "YourCollege"
                }
              ]
            }
          ],
          "scale": {
            "minReplicas": 1,
            "maxReplicas": 3
          }
        }
      }
    }
  ]
}

	Managed Environment:

Defines the managed environment for Azure Container Apps in the eastus region and p rovides the necessary environment for deploying container apps.

	MySQL Flexible Server:

Configures the MySQL Flexible Server with specified login credentials, storage configuration, and version, and provides the database backend required by the TempConverter application.

	Container App for TempConverter:

Configures the container app, including environment variables, ingress settings, and scaling options and deploys the TempConverter application as a container app in Azure.

	Deployment of the Application Using the Template

	Create Resource Group

az group create --name tempconverter-rg --location eastus

This command creates a new resource group named tempconverter-rg in the eastus region.

 
Image 13 Screenshot of the resource group "tempconverter-rg" being created in Azure.

	Deploy the Resources Using the JSON Template:

az deployment group create --resource-group tempconverter-rg --template-file azure-deployment.json

Deploys all resources defined in the azure-deployment.json template to the tempconverter-rg resource group and automates the deployment of the TempConverter application and its dependencies.

 
Image 14 Screenshot of deployed resources

	Accessing the Application

Upon completion of the deployment, the TempConverter application can be accessed via the external IP or URL provided by the Azure Container Apps environment.

https://tempconverter-app.whitetree-5e3aa089.eastus.azurecontainerapps.io

	Deploying TempConverter with OpenShift

In this section, we will discuss the deployment of the TempConverter application using OpenShift, a powerful Kubernetes-based container orchestration platform. This deployment will demonstrate how to leverage OpenShift’s features for managing containerized applications, ensuring scalability, reliability, and ease of management.

	Setting Up OpenShift Environment

Prerequisites:
	An OpenShift cluster (can be a local setup using Minishift or a cloud-based OpenShift instance)
	OpenShift CLI (oc) installed and configured
	Access to the TempConverter Docker image on Docker Hub

	Login to OpenShift:

oc login -u developer -p developer https://api.crc.testing:6443

Log into your OpenShift cluster using the oc command:
	oc login: This command logs you into the OpenShift cluster.
	-u developer: Specifies the username as developer.
	-p developer: Specifies the password as developer

	Create a New Project:

oc new-project tempconverter

Create a new project in OpenShift to keep your deployment isolated:
	oc new-project: This command creates a new project (namespace) in OpenShift.
	tempconverter: The name of the project being created.

	Creating the Deployment Configuration

YAML files are used to define the resources needed for deploying the TempConverter application and its MySQL database.

	MySQL Deployment Configuration:

A file named mysql-deployment.yaml is created with the following content:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:8
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: "rootpassword"
        - name: MYSQL_DATABASE
          value: "tempconverter"
        - name: MYSQL_USER
          value: "student"
        - name: MYSQL_PASSWORD
          value: "studentpassword"
        ports:
        - containerPort: 3306

Explanation:
	apiVersion: Specifies the API version (apps/v1) for creating a deployment.
	kind: Indicates that this resource is a Deployment.
	metadata: Contains metadata about the deployment, such as the name (mysql).
	spec: Specifies the desired state of the deployment, including the number of replicas, selectors, and template.
	containers: Defines the container to be deployed, including the name, image, environment variables, and ports.

	MySQL Service Configuration:

A file named mysql-service.yaml is created with the following content:


apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  selector:
    app: mysql
  ports:
  - protocol: TCP
    port: 3306
    targetPort: 3306

Explanation:
	apiVersion: Specifies the API version (v1) for creating a service.
	kind: Indicates that this resource is a Service.
	metadata: Contains metadata about the service, such as the name (mysql).
	spec: Specifies the desired state of the service, including the selector and ports.
	selector: Matches the labels defined in the MySQL deployment to connect the service to the correct pods.
	ports: Defines the port mappings for the service.


	TempConverter Deployment Configuration:

A file named tempconverter-deployment.yaml is created with the following content:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: tempconverter
spec:
  replicas: 1
  selector:
    matchLabels:
      app: tempconverter
  template:
    metadata:
      labels:
        app: tempconverter
    spec:
      containers:
      - name: tempconverter
        image: docker.io/pavaogusic/tempconverter:latest
        env:
        - name: DB_USER
          value: "student"
        - name: DB_PASS
          value: "studentpassword"
        - name: DB_HOST
          value: "mysql"
        - name: DB_NAME
          value: "tempconverter"
        ports:
        - containerPort: 5000

Explanation:
	apiVersion: Specifies the API version (apps/v1) for creating a deployment.
	kind: Indicates that this resource is a Deployment.
	metadata: Contains metadata about the deployment, such as the name (tempconverter).
	spec: Specifies the desired state of the deployment, including the number of replicas, selectors, and template.
	containers: Defines the container to be deployed, including the name, image, environment variables, and ports.
 


	TempConverter Service Configuration:

apiVersion: v1
kind: Service
metadata:
  name: tempconverter
spec:
  selector:
    app: tempconverter
  ports:
  - protocol: TCP
    port: 5000
    targetPort: 5000
  type: LoadBalancer

Explanation:
	apiVersion: Specifies the API version (v1) for creating a service.
	kind: Indicates that this resource is a Service.
	metadata: Contains metadata about the service, such as the name (tempconverter).
	spec: Specifies the desired state of the service, including the selector, ports, and type.
	selector: Matches the labels defined in the TempConverter deployment to connect the service to the correct pods.
	ports: Defines the port mappings for the service.
	type: Specifies the service type as LoadBalancer to expose the application externally.

 
Image 15 A screenshot displaying the list of all added template YAML files within the project directory.

	Deploying the Resources

Deploy MySQL:

oc apply -f mysql-deployment.yaml
oc apply -f mysql-service.yaml


To apply the MySQL deployment configuration and create the MySQL deployment, use the command oc apply -f mysql-deployment.yaml. Next, to apply the MySQL service configuration and create the MySQL service, use the command oc apply -f mysql-service.yaml.

Deploy TempConverter:

oc apply -f tempconverter-deployment.yaml
oc apply -f tempconverter-service.yaml

To apply the TempConverter deployment configuration and create the TempConverter deployment, use the command oc apply -f tempconverter-deployment.yaml. Next, to apply the TempConverter service configuration and create the TempConverter service, use the command oc apply -f tempconverter-service.yaml.

	Accessing the Application

After deploying the services, the TempConverter application can be accessed through the LoadBalancer service. The exact URL will depend on the OpenShift setup. The external IP or hostname can be found using:

oc get services

oc get services: Lists all services in the current project, showing details such as the external IP or hostname of the TempConverter service.

Opening a web browser and navigating to the provided URL will allow access to the TempConverter application.

	Conclusion

In conclusion, the project successfully achieved the goal of containerizing and deploying the TempConverter application across multiple platforms. By utilizing Podman for local containerization and Docker Hub for image repository management, the project ensured that the application is packaged with all necessary dependencies, enhancing its portability and consistency. The deployment on Azure Container Apps showcased the benefits of a serverless and scalable container environment, while the implementation on OpenShift demonstrated the effective management of Kubernetes-based orchestration. Each deployment step was meticulously documented, including environment setup, database integration, and application configuration, ensuring a clear and comprehensive guide for future projects. This project not only provided a hands-on experience with modern DevOps tools and practices but also highlighted the significant improvements in application scalability, reliability, and ease of management brought by containerization and orchestration technologies.

 

	References

[1] https://docs.docker.com/reference/dockerfile/, (2024). Docker, Accessed on: 2024-06-14
[2]	https://learn.microsoft.com/en-us/cli/azure/, (2024). Azure, Accessed on: 2024-06-14
[3]	https://docs.openshift.com/container-platform/4.10/welcome/index.html/, (2024). Red Ha OpenShift, Accessed on: 2024-06-14
[4]	https://github.com/jstanesic/tempconverter/, (2024). Github, Accessed on: 2024-06-14

