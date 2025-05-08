# Terraform-azure-pocs




##  main.tf
This file defines the resource to be created—in this case, an Azure Resource Group.

h
Copy
Edit
resource "azurerm_resource_group" "rg" {
  name     = var.resource_group_name
  location = var.location
}
📄 provider.tf
This configures the AzureRM provider. It uses Azure CLI credentials (az login) and takes the subscription ID from input.

hcl
Copy
Edit
provider "azurerm" {
  features {}

  subscription_id = var.subscription_id
  use_cli         = true
}
📄 variables.tf
This declares the input variables used in the configuration.

hcl
Copy
Edit
variable "resource_group_name" {
  description = "Name of the Azure Resource Group"
  type        = string
}

variable "location" {
  description = "Azure Region for the Resource Group"
  type        = string
}

variable "subscription_id" {
  description = "Azure Subscription ID"
  type        = string
}
📄 terraform.tfvars
This file provides values for the resource_group_name and location variables.

hcl
Copy
Edit
resource_group_name = "my-resource-group"
location            = "East US"
🔒 Note: We intentionally do not provide subscription_id here. It'll be passed via the shell.

✅ Steps to Deploy
1. 🔐 Authenticate with Azure CLI
Make sure you're logged in:

bash
Copy
Edit
az login
And select the desired subscription (optional if already set):

bash
Copy
Edit
az account set --subscription "e351298b-7dda-4e61-b6fe-629cd547028c"
2. 🌍 Export Subscription ID as an Environment Variable
This sets your subscription ID for Terraform use.

bash
Copy
Edit
export subscription_id="e351298b-7dda-4e61-b6fe-629cd547028c"
3. 🛠️ Initialize Terraform
bash
Copy
Edit
terraform init
4. 🔍 Preview the Plan
Pass the exported variable during plan:

bash
Copy
Edit
terraform plan -var="subscription_id=$subscription_id"
5. 🚀 Apply the Configuration
Create the resource group:

bash
Copy
Edit
terraform apply -var="subscription_id=$subscription_id"
Confirm when prompted.

6. 🧹 Destroy the Resource Group
Clean up resources:

bash
Copy
Edit
terraform destroy -var="subscription_id=$subscription_id"
✅ Expected Output
You'll see output like:

bash
Copy
Edit
azurerm_resource_group.rg: Creating...
azurerm_resource_group.rg: Creation complete after 2s [id=/subscriptions/xxx/resourceGroups/my-resource-group]
