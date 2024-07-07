[![Continuous Integration](https://github.com/datkenry29/uda-proj2/actions/workflows/main.yaml/badge.svg)](https://github.com/datkenry29/uda-proj2/actions/workflows/main.yaml)

[![Build Status](https://dev.azure.com/datquoc292001/uda-proj2/_apis/build/status%2Fdatkenry29.uda-proj2?branchName=master)](https://dev.azure.com/datquoc292001/uda-proj2/_build/latest?definitionId=3&branchName=master)

# Overview

This project involves building a Continuous Integration and Continuous Delivery (CI/CD) pipeline using Azure DevOps. The main goal is to automate the deployment of a Python application to Azure App Services, ensuring the application is tested and delivered efficiently and reliably. The project demonstrates core DevOps principles such as Infrastructure as Code (IaC), Continuous Integration, and Continuous Delivery, leveraging tools like Azure Cloud Shell, GitHub Actions, and Azure Pipelines.

## Project Plan

- **Trello Board**:
  ![Alt text](board.png)
- **Project Plan Spreadsheet**:
  ![Alt text](table.png)
  The spreadsheet includes:
  - Quarterly and yearly plan
  - Weekly deliverables
  - Estimates of difficulty and time for each task

The Trello board contains:

- Cards for key tasks
- A simple board-based flow: To Do, In Progress, Done

## Instructions

### Architectural Diagram

![Architectural Diagram](diagram.png)

The architectural diagram shows how key parts of the system interact, including the CI/CD pipeline, Azure services, and the deployed application.

### Running the Python Project

1. **Clone the Project into Azure Cloud Shell**
   - Open Azure Cloud Shell.
   - Create a ssh key
     ```
     ssh-keygen -t rsa
     ```
     ![Alt text](screenshots/image.png)
   - Copy the public key to your Github Account:
     ![Alt text](screenshots/image-1.png)
   - Clone the repository:
     ```
     git clone https://github.com/datkenry29/uda-proj2.git
     ```
     ![Alt text](screenshots/image-2.png)
   - Navigate to the project directory:
     ```
     cd uda-proj2/webapp/flask-sklearn
     ```
     ![Alt text](screenshots/image-3.png)
2. **Run Tests in Azure Cloud Shell**
   - Create Python Virtual Enviroment to run your application
     ```
     python3 -m venv ./.venv
     ```
     ![Alt text](screenshots/image-4.png)
   - Install dependencies and run the tests using the Makefile:
     ```
     make all
     ```
     ![Alt text](screenshots/image-5.png)
     ![Alt text](screenshots/image-6.png)
   - Run application
     ```
     export FLASK_APP=app.py
     flask run
     ```
     ![Alt text](screenshots/image-7.png)
   - Above step would launch a Python Virtual Environment and would run the application. Launch a new Azure Cloud shell session and test the application by running the make_prediction.sh script
     ```
     ./make_prediction.sh
     ```
     ![Alt text](screenshots/image-8.png)
   - `CTRL-C` to stop the Flask application
   - To deactivate the virtual environment run `deactivate`

### Azure App Service

1. **Create new Resource Group**
   - Run command below:
     ```
     az group create --name uda-proj2-rg --location eastus
     ```
     ![Alt text](screenshots/image-9.png)
2. **Deploy the Application using Azure CLI**

   - Deploy the application to Azure App Service:

     ```
     az webapp up --sku F1 --name flask-ml-uda-proj2 --resource-group uda-proj2-rg
     ```

     ![Alt text](screenshots/image-10.png)

     The Azure CLI commands in a Bash script called commands.sh file in the GitHub repo contains the steps Set up Azure CLI and Deploy Application

   - Our application will be deployed and available at https://${app-name}azurewebsites.net default port is 443
     ![Alt text](screenshots/image-11.png)
   - Azure app service from the Azure portal
     ![Alt text](screenshots/image-12.png)

### Github Action

1. **Successful deploy of the project in GitHub Actions**
   ![Alt text](screenshots/image-13.png)
   ![Alt text](screenshots/image-14.png)

### Azure DevOps

1. **Set Up Azure Pipelines for Continuous Delivery**
   - Navigate to [dev.azure.com](dev.azure.com) and sign in. Then create new project if not exitst.
     ![Alt text](screenshots/image-15.png)
   - Once the project is created, from the new project page, select Project settings from the left navigation. On the Project Settings page, select Pipelines > Service connections, then select New service connection.
     ![Alt text](screenshots/image-16.png)
   - In the New Service Connections dialog, select Azure Resource Manager from the dropdown.
     ![Alt text](screenshots/image-17.png)
   - In `New Azure service connection` dialogue box, select `Workload Identity federation (automatic)`.
     ![Alt text](screenshots/image-18.png)
   - Press `Next` and do following steps:
     - Select scope level as `Subscription`
     - You might need to log in
     - Pick the Resource Group of the Azure Web App deployed
     - Input a valid Service Connection Name
     - Need to check the box Grant Access Permissions to all pipelines
     - Save
       ![Alt text](screenshots/image-19.png)
2. **Azure Pipeline App**

   - From your project page left navigation, navigate to `Pipelines` -> `Create Pipelines`
     ![Alt text](screenshots/image-20.png)
   - In the New Pipeline screen -> Select GitHub as Repo -> Select the Project
     ![Alt text](screenshots/image-21.png)
     ![Alt text](screenshots/image-22.png)
   - In tab `Configure`, select `Python to Linux Azure Webapp` -> Select the deployed app -> Validate and configure
   - ![Alt text](screenshots/image-23.png)

   - Create an Azure Pipeline in Azure DevOps.
   - Configure the pipeline to build and deploy the application automatically.
     ![Alt text](screenshots/image-24.png)
     ![Alt text](screenshots/image-25.png)
     ![Alt text](screenshots/image-26.png)
     ![Alt text](screenshots/image-27.png)

3. **Run a Load Test with Locust**

   - Install Locust and run the load test:
     ```
     locust -f locustfile.py
     ```
     ![Alt text](screenshots/image-30.png)
     ![Alt text](screenshots/image-31.png)

4. **Test the Deployed Application**

   - Run a prediction from the deployed application:

   ```
   ./make_predict_azure_app.sh
   ```

   ![Alt text](screenshots/image-28.png)

5. **Stream Log Files**
   - Stream the log files from the deployed application:
     ```
     az webapp log tail --name flask-ml-uda-proj2 --resource-group uda-proj2-rg
     ```
     ![Alt text](screenshots/image-29.png)

## Enhancements

In the future, the project can be improved by:

- Integrating automated security scans into the CI/CD pipeline.
- Implementing advanced monitoring and alerting with Azure Monitor.
- Enhancing the application with additional features and functionalities.
- Optimizing the CI/CD pipeline for performance and scalability.

## Demo

Watch the project demo on YouTube: [Project Demo Screencast](https://youtu.be/0B0m2Ut7wMk)

[![VIDEO](https://img.youtube.com/vi/0B0m2Ut7wMk/0.jpg)](https://www.youtube.com/watch?v=0B0m2Ut7wMk)

The screencast demonstrates:

- The working Azure Cloud Shell environment for Continuous Integration.
- The GitHub Actions build process.
- Successful deployment using Continuous Delivery on the Azure platform.
- A machine learning prediction returning a JSON payload.
