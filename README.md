# sre-lab-module-2

1. Create Azure Container with sample image
2. Get SubId to create a Service Principal 6611e7b5-8b75-49a9-9fac-f565cd47abee
3. az ad sp create-for-rbac --name "srelab2" --role contributor --scopes /subscriptions/6611e7b5-8b75-49a9-9fac-f565cd47abee --sdk-auth

NewSP in Github Project 

{
  "clientId": XXXX
  XXXXXXXXXXXXXXXX
  XXXXXXXXXXXXXXXX
  "managementEndpointUrl": XXXX
}


docker pull mcr.microsoft.com/mcr/hello-world
docker tag mcr.microsoft.com/mcr/hello-world srelab2/samples/hello-world

docker build -t mcr.microsoft.com/mcr/hello-world
