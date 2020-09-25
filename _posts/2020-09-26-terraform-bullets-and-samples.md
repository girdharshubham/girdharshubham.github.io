---  
title: "Terraform bullet points and samples"
comments: true
category: ["iac","terraform"]
---  
This post contains all the content exercised while learning [Terraform](https://www.terraform.io/)  
    
# What is Terraform  
* IaC offering by Hashicorp.  
* Build, change, and manage infrastructure in a safe, repeatable way.  
* Uses HCL - HashiCorp Configuration Language.
  
# What is Infrastructure as Code  
* Process of managing infrastructure(**Hadware**) in a file or files rather than manually configuring resources in a user interface.   
* Declarative - better than CLIs and SDKs  
* Resurces on any provider - Docker, GCP, Azure, AWS.  
  
# Flow  
* Identify and confirm resources that need to be created.  
* Create the configuration file - use HCL.  
* Initialize **Terraform** - This will aut download the corresponding provider. Command: `terraform init`  
* Terraform creates a *plan* that helps you verify the resources that are going to be created on the provider. Command - `terraform plan`  
* Apply - to create real resources and `.tfstate` file. Command - `terraform apply`  
  
## More on `.tfstate`  
This is a state file that terraform creates when the project is first initialized. It is on the basis of this file that plans and infrastrucure are created. This file is refreshed prior to any operation on the infrastructure. This is kinda your source of truth to measure configuration changes i.e. if you make a change, `terraform` compares that change with the state file to identify what resource is going to be changed.  