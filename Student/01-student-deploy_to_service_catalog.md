# Challenge 1: Deploy your first Azure Managed Application with Service Catalog

## Introduction

In this challenge, you will deploy a Managed Application in the Azure portal. You’ll do this using an Azure service called the “Service Catalog.”

## Description

Deploying an Managed Application consist of two main steps, creating the Service Catalog Managed Application Definition and creating the Application. Service Managed Application Definition consist two files:
   - mainTemplate.json - an ARM template for resources beeing deployed
   - CreateUIDefinition.json - file used to define the user interface when creating Managed Applictaion
   
Both of those files are provided for this challenge.

1. Before You begin in Azure portal create a new resource group, storage account and a storage container. Use the following values for storage account properties:
    - Storage account name > Some unique value
    - Basics > Replication > Locally Redundant Storage (LRS)
    - Networking > Network Connectivity > Public endpoint (all networks)
    - For the rest > Leave the defauls

2. Upload packaged application to storage container

3. Create a **Service Catalog Managed Application Definition** in the same resource group (*Using Portal will be easier). 

4. Create a **Service Catalog Managed Application** from it in a separate resource group (look for "Deploy from definition" button).

Once the deployment is complete connect to the virtual machine via RDP. After verifing the remote connection, you may shut down the machine so you won’t be charged for VM uptime.

## Success Criteria

1. You have a resource group with a storage account holding a ZIP file with your application JSONs.
2. You have a resource group with your Managed Application deployed in Service Catalog.
3. You can successfully RDP into your new virtual machine.

## Learning Resources

- [Create a storage account](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal)
- [Announcing general availability of Azure Managed Applications Service Catalog](https://azure.microsoft.com/en-us/blog/announcing-general-availability-of-azure-managed-applications-service-catalog/)
- [What are ARM templates?](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/overview)
- [CreateUiDefinition.json for Azure managed application's create experience](https://docs.microsoft.com/en-us/azure/azure-resource-manager/managed-applications/create-uidefinition-overview)
- [Create and publish a Service Catalog Managed Application Definition](https://docs.microsoft.com/en-us/azure/azure-resource-manager/managed-applications/publish-service-catalog-app?tabs=azure-powershell)
