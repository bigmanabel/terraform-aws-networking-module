# Terraform Local Networking Module

This project demonstrates the use of a custom Terraform module for deploying AWS networking resources, including a VPC and configurable subnets (public/private) using local modules.

## Features
- Create a VPC with a user-defined CIDR block
- Create multiple subnets with user-defined CIDR blocks and availability zones
- Mark subnets as public or private
- Automatically deploy an Internet Gateway (IGW) if at least one subnet is public
- Associate public subnets with a public route table

## Prerequisites
- [Terraform](https://www.terraform.io/downloads.html) >= 1.0
- AWS credentials configured (via environment variables, AWS CLI, or shared credentials file)

## Usage
Clone this repository and initialize Terraform:

```sh
terraform init
terraform plan
terraform apply
```

## Module Inputs
- `vpc_config` (map):
  - `cidr_block` (string): The CIDR block for the VPC
  - `name` (string): Name tag for the VPC
- `subnet_config` (map):
  - Each key is a subnet name, value is a map with:
    - `cidr_block` (string): The CIDR block for the subnet
    - `az` (string): The availability zone
    - `public` (bool, optional): Whether the subnet is public (default: false)

## Module Outputs
- `vpc_id`: The ID of the created VPC
- `public_subnets`: List of public subnet IDs
- `private_subnets`: List of private subnet IDs

## Example
```
module "networking" {
  source = "./modules/networking"

  vpc_config = {
    cidr_block = "10.0.0.0/16"
    name       = "example-vpc"
  }

  subnet_config = {
    subnet_1 = {
      cidr_block = "10.0.0.0/24"
      az         = "us-east-1a"
    }
    subnet_2 = {
      cidr_block = "10.0.1.0/24"
      public     = true
      az         = "us-east-1b"
    }
  }
}
```

---

## Development Checklist

1. [DONE] Create a VPC with a given CIDR block
2. Allow the user to provide the configuration for multiple subnets:
   2.1 [DONE] The user should be able to provide CIDR blocks
   2.2 [DONE] The user should be able to provide AWS AZ
   2.3 [DONE] The user should be able to mark a subnet as public or private
        2.3.1 If at least one subnet is public, we need to deploy an IGW
        2.3.2 We need to associate the public subnets with a public route table
