# Terraform-azure-pocs


## Resource Group creation

###  main.tf

```bash
resource "azurerm_resource_group" "rg" {
  name     = var.resource_group_name
  location = var.location
```

### provider.tf
```bash
provider "azurerm" {
  features {}

  subscription_id = var.subscription_id
  
}
```
### variables.tf
```bash
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
```
### terraform.tfvars

```bash
resource_group_name = "my-resource-group"
location            = "East US"
```
#### Note: We intentionally do not provide subscription_id here. It'll be passed via the shell.

### Steps to Deploy

1.  Authenticate with Azure CLI

```bash
az login
```

2.  Export Subscription ID as an Environment Variable
This sets your subscription ID for Terraform use.

```bash
export subscription_id="<sub id>"
```
3.  Initialize Terraform
4.  
```bash
terraform init
```
4.  Preview the Plan


```bash
terraform plan -var="subscription_id=$subscription_id"
```
5.  Apply the Configuration
Create the resource group:


```bash
terraform apply -var="subscription_id=$subscription_id"
```

6.  Destroy the Resource Group
Clean up resources:

```bash
terraform destroy -var="subscription_id=$subscription_id"
```

7.  Expected Output
You'll see output like:

```bash
azurerm_resource_group.rg: Creating...
azurerm_resource_group.rg: Creation complete after 2s [id=/subscriptions/xxx/resourceGroups/my-resource-group]
```
