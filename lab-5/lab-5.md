# Lab 5: Implementing custom providers

Azure Managed Applications Workshop

## Lab overview

In this lab you will customize and deploy a managed application with functionality to manage and store custom resources.

# Exercise 1: Deploy the function package

In this exercise you will deploy a packaged Azure function to a storage account, making it ready for deployment by an ARM template.

## Create a storage account

1. Create a new Azure storage account
2. Put it in a new resource group named "AMALab5"

## Upoad the function application

1. In the blob storage of your new storage account, create a container named "function".
2. Upload the `lab-5/begin/functionpackage.zip` file to the new container.
3. Click on the ZIP file and then copy the URL for that blob.
4. Paste the URL to a location for use later.

# Exercise 2: Set up the ARM template

In this exercise you will add the custom provider resources to the `mainTemplate.json` ARM template for a managed application. 

## Add the function package URL

1. Open the `lab-5/begin/mainTemplate.json` file for editing.
2. Find the following JSON.

```json
"zipFileBlobUri": {
    "type": "string",
    "defaultValue": "CHANGE_THIS",
    "metadata": {
        "description": "The Uri to the uploaded function zip file"
    }
}
```
3. Replace `CHANGE_THIS` with the URL of the function app package you copied in the previous step.

## Add the custom provider resource

1. Find the resources section and note there are already 3 resources defined.
2. Add the following JSON to the array of resources so the ARM template will create the custom resource provider.

```json
{
    "apiVersion": "[variables('customrpApiversion')]",
    "type": "Microsoft.CustomProviders/resourceProviders",
    "name": "[variables('customProviderName')]",
    "location": "[parameters('location')]",
    "properties": {
        "actions": [],
        "resourceTypes": []
    },
    "dependsOn": [
        "[concat('Microsoft.Web/sites/',parameters('funcname'))]"
    ]
}

```

## Add custom actions

1. Add the following actions to the `actions` array.

```json
{
    "name": "ping",
    "routingType": "Proxy",
    "endpoint": "[listSecrets(resourceId('Microsoft.Web/sites/functions', parameters('funcname'), 'HttpTrigger1'), '2018-02-01').trigger_url]"
},
{
    "name": "users/contextAction",
    "routingType": "Proxy",
    "endpoint": "[listSecrets(resourceId('Microsoft.Web/sites/functions', parameters('funcname'), 'HttpTrigger1'), '2018-02-01').trigger_url]"
}
```

## Add the custom resource type

1. Find the `resourceTypes` array.
2. Add the following resource type to the array.

```json
{
    "name": "users",
    "routingType": "Proxy,Cache",
    "endpoint": "[listSecrets(resourceId('Microsoft.Web/sites/functions', parameters('funcname'), 'HttpTrigger1'), '2018-02-01').trigger_url]"
}
```

# Exercise 3: Set up the view definition

In this exercise you will add the custom provider elements to the `viewDefinition.json` that defines the interface for the managed application. You will add UI elements that make use of the function application for customizing the managed application.

## Add the overview command

1. Open the `lab-5/begin/viewDefinition.json` file for editing.
2. Find the **overview** section and add the following JSON after the **description** to declare a command. This command will appear at the top of the managed application.

```json
"commands": [
    {
        "displayName": "Ping Action",
        "path": "/customping",
        "icon": "LaunchCurrent"
    }
]
```
The **overview** view should now look like this.

```json
{
    "kind": "Overview",
    "properties": {
        "header": "Welcome to ...",
        "description": "This Managed application ...",
        "commands": [
            {
                "displayName": "Ping Action",
                "path": "/customping",
                "icon": "LaunchCurrent"
            }
        ]
    }
}
```

## Add the custom resources UI

1. Find the `"resourceType": "users"` value.
2. Paste the following `createUIDefinition` section below `"resourceType": "users"`.

```json
"createUIDefinition": {
    "parameters": {
        "steps": [
            {
                "name": "add",
                "label": "Add user",
                "elements": [
                    {
                        "name": "name",
                        "label": "User's Full Name",
                        "type": "Microsoft.Common.TextBox",
                        "defaultValue": "",
                        "toolTip": "Provide a full user name.",
                        "constraints": {
                            "required": true
                        }
                    },
                    {
                        "name": "location",
                        "label": "User's Location",
                        "type": "Microsoft.Common.TextBox",
                        "defaultValue": "",
                        "toolTip": "Provide a Location.",
                        "constraints": {
                            "required": true
                        }
                    }
                ]
            }
        ],
        "outputs": {
            "name": "[steps('add').name]",
            "properties": {
                "FullName": "[steps('add').name]",
                "Location": "[steps('add').location]"
            }
        }
    }
}
```
Note the above is a fully formed createUIDefinition as you might find in a dedicated file of the same name.

## Add commands

1. After the `createUIDefinition` property, add the following JSON to define the command that will be executed.

```json
"commands": [
    {
        "displayName": "Custom Context Action",
        "path": "users/contextAction",
        "icon": "Start"
    }
]
```
## Add columns for layout

1. Under the commands array, add the following JSON to define the layout for showing any custom resources.

```json
"columns": [
    {
        "key": "properties.FullName",
        "displayName": "Full Name"
    },
    {
        "key": "properties.Location",
        "displayName": "Location",
        "optional": true
    }
]
```
You now have a fully formed `viewDefinition.json`.    


# Exercise 4: Deploy the managed application package

In this exercise you will add the managed application artifacts to a storage account, making them ready for deployment.

## Create the deployment package

ZIP the following files into a ZIP file with the files at the root.

- createUiDefinition.json
- mainTemplate.json
- viewDefinition.json

## Upload the package to the storage account

1. In the storage account you created earlier, create a new container named "arm".
2. Upload your ZIP file into this container.
3. Copy the URL for this blob and paste it somewhere so you have easy access to it later.

# Exercise 5: Deploy the managed application

In this exercise you will create a new Managed Application Definition and use it to deploy the managed application.

## Create the managed application definition

1. Create a new "Service catalog managed application definition" in the Azure portal.
2. Fill it out as shown below.

![Service catalog managed application definition](./images/01.png)
![Service catalog managed application definition](./images/02.png)

## Deploy the managed application

After creating the AMA Definition, deploy it as a managed application.

# Exercise 6: Using custom resources

In this exercise you will use the custom provider functionality you added to the managed appliciation.

## Running the ping action

1. Navigate to the newly created managed application.
2. At the top of the page, find the button labeled "Ping Action."

This button was defined in the Overview section of `viewDefinition.json` as a command.

3. Click the button to send a ping to the function application that was deployed. A successful return looks like the following.

![Successful ping](./images/03.png)

## Managing users

1. In the left hand menu of the managed application, find the enu item labeled "Users" under the "Resources" section.
2. Click the "Users" button. You are directed to a screen showing a list of user names. The list is currently empty.

> This screen layout was defined by the `columns` section of `viewDefinition.json`.

### Add new users

When creating a new user, you are actually creating a new Azure resource.

1. Click Add

> This screen was defined by the `createUIDefinition`  section of `viewDefinition.json`.

2. Enter a new user's full name and location.
3. Review the new resource to ensure it passes validation. and submit the new resource.

> You can now see the new Azure resource in the list of users.

4. Add another user.

### Inspecting the user resource id

1. Click on one of the users in the list.

> This resource overview shows the containing resource group and subscription like any other resource in Azure.

2. Click the "Properties" menu item on the left.
3. Scroll down to inspect the "Resource ID" property.

> This resource ID demostrates this resource is addressable via Azure APIs like any other resource in Azure.

### Inspecting user storage

Now that we've seen this is an Azure resource, let's look at where it is stored.

1. Navigate to the resource's managed resource group.
2. Open the storage account that was created in the resource group.
3. From the left hand menu, open the "Storage Exlporer" menu item.
4. Expand TABLES as shown below.

![Tables](./images/04.png)

5. Click on the customResource table.
6. Inspect the custom resources that were created by the resource provider.
7. Scrolling to the right shows the DATA column, which contains the JSON representation of each resource you created.

### Deleting a user

1. Navigate back to the managed application you created.
2. Click the "Users" menu item on the left.
3. Click on a user resource.
4. On the resource's Overview page, click the Delete button and confirm deletion.

You now only see one user resource.

You may go back to the storage table to see the record for that rescource no longer exists.

# Congratulations

In this lab you used custom resource providers defined in ARM templates to create custom resources.