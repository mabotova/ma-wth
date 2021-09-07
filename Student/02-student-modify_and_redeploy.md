# Challenge 2: Modify the application

## Introduction

In this challenge, you will change the type of VM from Windows Server to Ubuntu, add a storage account to your application, and redeploy the application.

## Description

1.	Now you need to update the `createUiDefinition.json` file to create an installation experience that prompts the user for details needed to create an Azure Storage account. The values gathered will be input into the ARM template as parameters at install time.

> __Note:__ At any point in editing the `createUiDefinition.json` file, consider using the [Create UI Definition](https://portal.azure.com/?feature.customPortal=false#blade/Microsoft_Azure_CreateUIDef/SandboxBlade) sandbox in the Azure portal to check your sytax and functionality.

2. In your ARM template (mainTemplate.json) in the parameters section add the required parameters. 

3. In your ARM template make changes to create an Ubuntu image instead of Windows Server. 

4. In your ARM template add a new section to create a storage account.

5. Download a tool used in testing ARM templates and use it to test your mainTemplate.json and createUiDefinition.json files. Fix any errors reported by the tool.

    [ARM template test toolkit - Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/test-toolkit)

6. Package your new application and publish it in Service Catalog as a separate managed application.

7. Once the deployment is complete check that your Ubuntu virtual machine is up and running. After verifing, you may shut down the machine so you won’t be charged for VM uptime.

## Success Criteria

1. Your files passes the Template Test Toolkit without any errors.
2. You have a new resource group with your updated Managed Application deployed in Service Catalog.
3. The new Managed Application contains a storage account.
4. You can successfully SSH into your new Ubuntu virtual machine.

## Learning Resources

- <https://docs.microsoft.com/en-us/azure/azure-resource-manager/managed-applications/create-uidefinition-overview>
- <https://docs.microsoft.com/en-us/azure/azure-resource-manager/managed-applications/test-createuidefinition>
- <https://docs.microsoft.com/en-us/azure/azure-resource-manager/managed-applications/microsoft-storage-storageaccountselector>
- <https://docs.microsoft.com/en-us/azure/virtual-machines/linux/create-ssh-secured-vm-from-template>
- <https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/test-toolkit>
