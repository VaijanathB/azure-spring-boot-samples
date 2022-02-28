# Using Spring Cloud Stream Binder for Azure Event Hubs

This code sample demonstrates how to use the `Spring Cloud Stream Binder` for `Azure Event Hubs`.The
sample app has two operating modes.
One way is to expose a Restful API to receive string message, another way is to automatically provide string messages.
These messages are published to one `Event Hub` instance and then consumed by one consumer
endpoint from the same application.

## What You Will Build
You will build an application using `Spring Cloud Stream Binder` to send and receive messages for `Azure Event Hubs`.

## What You Need

- [An Azure subscription](https://azure.microsoft.com/free/)
- [Terraform](https://www.terraform.io/)
- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)
- [JDK8](https://www.oracle.com/java/technologies/downloads/) or later
- Maven
- You can also import the code straight into your IDE:
    - [IntelliJ IDEA](https://www.jetbrains.com/idea/download)

## Provision Azure Resources Required to Run This Sample
This sample will create Azure resources using Terraform. If you choose to run it without using Terraform to provision resources, please pay attention to:
> [!IMPORTANT]  
> If you choose to use a security principal to authenticate and authorize with Azure Active Directory for accessing an Azure resource
> please refer to [Authorize access with Azure AD](https://microsoft.github.io/spring-cloud-azure/current/reference/html/index.html#authorize-access-with-azure-active-directory) to make sure the security principal has been granted the sufficient permission to access the Azure resource.

### Authenticate Using the Azure CLI
Terraform must authenticate to Azure to create infrastructure.

In your terminal, use the Azure CLI tool to setup your account permissions locally.

```shell
az login
```

Your browser window will open and you will be prompted to enter your Azure login credentials. After successful authentication, your terminal will display your subscription information. You do not need to save this output as it is saved in your system for Terraform to use.

```shell
You have logged in. Now let us find all the subscriptions to which you have access...

[
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "home-Tenant-Id",
    "id": "subscription-id",
    "isDefault": true,
    "managedByTenants": [],
    "name": "Subscription-Name",
    "state": "Enabled",
    "tenantId": "0envbwi39-TenantId",
    "user": {
      "name": "your-username@domain.com",
      "type": "user"
    }
  }
]
```

If you have more than one subscription, specify the subscription-id you want to use with command below: 
```shell
az account set --subscription <your-subscription-id>
```

### Provision the Resources

After login Azure CLI with your account, now you can use the terraform script to create Azure Resources.

#### Run with Bash

```shell
# In the root directory of the sample
# Initialize your Terraform configuration
terraform -chdir=./terraform init

# Apply your Terraform Configuration
terraform -chdir=./terraform apply -auto-approve

```

#### Run with Powershell

```shell
# In the root directory of the sample
# Initialize your Terraform configuration
terraform -chdir=terraform init

# Apply your Terraform Configuration
terraform -chdir=terraform apply -auto-approve

```

It may take a few minutes to run the script. After successful running, you will see prompt information like below:

```shell


azurerm_resource_group.main: Creating...
azurerm_resource_group.main: Creation complete after 3s [id=/subscriptions/799c12ba-353c-44a1-883d-84808ebb2216/resourceGroups/rg-eventhubs-binder-nxatj]
azurerm_eventhub_namespace.eventhubs_namespace: Creating...
azurerm_storage_account.storage_account: Creating...
...
azurerm_storage_account.storage_account: Creation complete ...
azurerm_storage_container.storage_container: Creating...
azurerm_role_assignment.role_storage_account_contributor: Creating...
azurerm_storage_container.storage_container: Creation complete ...
azurerm_role_assignment.role_storage_blob_data_owner: Creating...
...
azurerm_role_assignment.role_storage_blob_data_owner: Creation complete ...
azurerm_role_assignment.role_storage_account_contributor: Creation complete ...
...
azurerm_eventhub_namespace.eventhubs_namespace: Creation complete ...
azurerm_eventhub.eventhubs: Creating...
azurerm_eventhub.eventhubs: Creation complete ...
...
azurerm_role_assignment.role_eventhubs_data_owner: Creation complete ...

Apply complete! Resources: 8 added, 0 changed, 0 destroyed.

Outputs:
...


```

You can go to [Azure portal](https://ms.portal.azure.com/) in your web browser to check the resources you created.

### Export Output to Your Local Environment
Running the command below to export environment values:

#### Run with Bash

```shell
source ./terraform/setup_env.sh
```

#### Run with Powershell

```shell
 . terraform\setup_env.ps1
```

## Run Locally

In your terminal, run `mvn clean spring-boot:run`.


```shell
mvn clean spring-boot:run
```

## Verify This Sample

1.  Verify in your app’s logs that similar messages were posted:

```shell
New message received: 'Hello world, 17' ...
Message 'Hello world, 17' successfully checkpointed
...
New message received: 'Hello world, 18' ...
Message 'Hello world, 18' successfully checkpointed
...
New message received: 'Hello world, 27' ...
Message 'Hello world, 27' successfully checkpointed

```

## Clean Up Resources
After running the sample, if you don't want to run the sample, remember to destroy the Azure resources you created to avoid unnecessary billing.

The terraform destroy command terminates resources managed by your Terraform project.   
To destroy the resources you created.

#### Run with Bash

```shell
terraform -chdir=./terraform destroy -auto-approve
```

#### Run with Powershell

```shell
terraform -chdir=terraform destroy -auto-approve
```