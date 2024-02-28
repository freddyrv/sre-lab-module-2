# sre-lab-module-2

#Pre-requisites

1. Have a github account, you need to copy github project to your repo, https://github.com/freddyrv/sre-lab-module-2.
2. Create an Azure free/trial account since is required to follow github actions azure integration.

#Azure Github Actions integration

1. Create an Azure Container App with sample image/or any prefered image. instructions: https://learn.microsoft.com/en-us/azure/container-apps/quickstart-portal
2. Get Azure Subsciption Id which will be required in next step to create a Service Principal.
3. Once you have the Azure SubId proceed to rename "resource" in the following command with either resource group name or container app, then execute command from Azure CLI, this will generate a new Service Principal which will be under subscription scope, "az ad sp create-for-rbac --name "resource" --role contributor --scopes /subscriptions/6611e7b5-8b75-49a9-9fac-f565cd47abee --sdk-auth
4. Copy and save the output from previous command since this secret will be used in the next step.

5. Go into the github project you have copied and generate a new secret, instructions can be found on the following link: https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions

    Content of the text should be similar to the example below, formatwise, set the secret name and save it, since it will be used in a future configuration step.
    
    {
      "clientId": XXXX
      XXXXXXXXXXXXXXXX
      XXXXXXXXXXXXXXXX
      "managementEndpointUrl": XXXX
    }

6. Create an Azure container Registry and follow instructions to install docker locally either on Windows/Mac/Linux,

For this example we will be using latest ubuntu image, you will need to run the following commands from Azure CLI.

  docker pull ubuntu:latest
  
  docker tag ubuntu:latest srelab2.azurecr.io/ubuntu:latest #replace srelab2.azurecr.io with your ACR name
  
  docker push srelab2.azurecr.io//samples/ubuntu:latest #replace srelab2.azurecr.io with your ACR name
  
  docker push srelab2.azurecr.io/ubuntu:latest #replace srelab2.azurecr.io with your ACR name

Once you have added latest ubuntu image to your Azure Container registry, integrated github actions with azure, let's edit the main.yaml to get ready for the deployment automation.

#Github action/worflow setup

1. Create a new workflow or edit current main.yaml for actions, where the following configurations needs to be done according to your Azure environment.

   name: Deploy Container App to Azure

on:
  push:
    branches: [ "main" ]
    paths:
      - "src/**"
  workflow_dispatch:

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest # For Linux, use ubuntu-latest
    environment: dev
    
    steps: 
    - name: 'Checkout GitHub Action'
      uses: actions/checkout@v3
    
    - name: Log in to Azure
      uses: azure/login@v1
      with:
        creds: ${{ secrets.NEWSP }} #replace NEWSP with azure credentials name
  
    - name: Build and deploy Container App
      uses: azure/container-apps-deploy-action@v1
      with:
        appSourcePath: ${{ github.workspace }}/src #app path
        acrName: srelab2 #ACR name
        containerAppName: srelab2 #Container App Name
        resourceGroup: srelab2 #Resource Group Name

2. Once you have replaced the required values, proceed to execute workflow wich would be automatically deployed to the Azure Container App.


  
