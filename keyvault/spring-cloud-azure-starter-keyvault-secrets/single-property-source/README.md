# Using Key Vault Secrets With Spring Cloud Azure Key Vault Secrets Starter

This sample illustrates the simplest usage of `spring-cloud-azure-starter-keyvault-secrets`. 
To learn all features, please refer to [reference doc](https://microsoft.github.io/spring-cloud-azure/current/reference/html/index.html#secret-management).

## What You Will Build

You will build an application that use `spring-cloud-azure-starter-keyvault-secrets` to retrieve secrets from [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).

## What You Need

- [An Azure subscription](https://azure.microsoft.com/free/)
- [Terraform](https://www.terraform.io/)
- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)
- [JDK8](https://www.oracle.com/java/technologies/downloads/) or later
- Maven
- You can also import the code straight into your IDE:
    - [IntelliJ IDEA](https://www.jetbrains.com/idea/download)

## Provision Azure Resources Required to Run This Sample

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
...
azurecaf_name.kv: Creating...
azurecaf_name.resource_group: Creating...
azurecaf_name.resource_group: Creation complete after ...
azurecaf_name.kv: Creation complete after 0s ...
azurerm_resource_group.main: Creating...
azurerm_resource_group.main: Creation complete after 4s ...
azurerm_key_vault.kv_account: Creating...
azurerm_key_vault.kv_account: Still creating... 
...
azurerm_key_vault.kv_account: Creation complete after 2m36s ...
azurerm_key_vault_secret.kv: Creating...
azurerm_key_vault_secret.kv: Creation complete after 8s ...

Apply complete! Resources: 5 added, 0 changed, 0 destroyed.

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

Start the application, you will see a log like this:

```text
sampleProperty: kv: sampleProperty: value
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