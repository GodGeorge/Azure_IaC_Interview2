####### THIS IS THE README FOR AZURE FOR A THREE TIER APPLICATION ########

This involves the deployment of IaC in Azure with the use of ARM Templates and Biceps. It involves a 3 tier app 
stack involving a Web Application Gateway posting a HTTP request to a Payments Web API which posts a message on
Azure Event Hub

######## Application Gateway #######################
We deployed a LAMP Stack on Azure and the details as per below

1) Create a Resource Group on Azure in our case we named it "bicep-dev-sa-rg"

You can deploy as per the json view with changes as per Azure subscription.

It was assumed that since M-KOPA users on the MobileBux have credentials and unique details all these details might be 
captured on the Mysql DB on the LAMP stack and thus the reason for the storage deployment together with the Apache Server
for pushing the HTTP post to the Payment Web Api.

https://github.com/Azure/azure-quickstart-templates/tree/master/demos/lamp-app

we tested by performing curl as per the readme to post a http request to the API.

$ curl -v -X POST -H 'Content-Type:application/json' -d '{\"Amount\": 100}' http://localhost:5000/payment_notifications

Kindly change the localhost to the IP generated in Azure for the Azure Container Instance of the Payment Api.


######### Event Hubs ############################
For sending payment notification messages to the Event Hub.

Since to test the WebApi we needed to change the Event Hub Name and Entry Point, we had to create the Azure Event Hub
first.

##### Dependencies for WebApi #######
 "ConnectionString": "Endpoint=sb://hireeventhubns.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=MD4bf4cbxJiRZ0DPgDBYSTQwNgMqpndc5nSPUWhZUkk=",
    "EventHubName": "hireeventhubns"

Details for the Event Deployment and Templates as per git repository.

######### Payments Notifications Web Api #######

Code was provided to ensure that the API was working fine together with a read me.

As per API you had to make changes on the appsettings json where the endpoint and name had to be replaced.

For making changes we used Visual Studio, .NET Framework and NuGet for testing the WebApi confirm if the metioned softwares
are available on the laptop.

Run the dotnet run and dotnet build commands as per readme for the code shared...it should be a success.

Once this is done we need to build a docker image so that we can create an Azure Container Instance.

Once the API runs successful we need to create a Dockerfile on VS code as per sample below.



Make sure you have Docker Desktop installed in your laptop and running fine.

Inside the parent directory for the WebApi perform the docker run command as per sample below

docker build --rm -t <tag>:latest . -f .\Dockerfile.dockerfile

Make sure the tag is the same name as the public repos on docker hub.

Once done, then check if its available on your local repo thro

docker image ls

Push the docker image 

docker push <tag>

Once completed then we deploy an Azure Container Instance pointing the image to the Payments Web API  as per  below.

https://hub.docker.com/repository/docker/wanjala/mkopa-testing

Make sure the PaymentNotifications web api to be working by checking the messages on events hub



All the code is in:

For all resources for IaC

https://github.com/GodGeorge/Azure_IaC_Interview2






 

