# Terraform-azure-pocs


## Resource Group & Virtual network creation

###  main.tf

```bash
resource "azurerm_resource_group" "rg" {
  name     = var.resource_group_name
  location = var.location
}

resource "azurerm_virtual_network" "vnet" {
  name                = var.vnet_name
  address_space       = var.address_space
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
}
```

### provider.tf
```bash
provider "azurerm" {
  features {}

  subscription_id = "${var.subscription_id}"
  
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

variable "vnet_name" {
  description = "Name of the Azure Virtual Network"
  type        = string
}

variable "address_space" {
  description = "Address space for the Virtual Network"
  type        = list(string)
}
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

vnet_name           = "my-vnet"
address_space       = ["10.0.0.0/16"]

```
#### Note: We intentionally do not provide subscription_id here. It'll be passed via the shell.

### Outputs.tf

```bash

output "vnet" {
  value = azurerm_virtual_network.vnet
  
}
output "resource_group_name" {
  value = azurerm_resource_group.rg
}
```

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
