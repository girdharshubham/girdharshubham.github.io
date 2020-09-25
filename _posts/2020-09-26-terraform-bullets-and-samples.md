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
  
# Build Infrastructure - AWS  
## Step 1. Create an AWS account and get security credentials for it - AWSAccessKeyId and AWSSecretKey  
## Step 2. Download and install aws cli  
{% highlight shell %}
$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
{% endhighlight %}

## Step 3. Write main.tf
**main.tf**
{% highlight hcl %}
terraform {
    required_providers {
        aws = {
            source          = "hasicorp/aws"
            version         =  "~> 3.0"
        }
    }
}

provider "aws" {} # Going to provide the credentials as env vars

resource "aws_instance" "ami_instance" {
    ami                     = "ami-830c94e3"
    instance_type           = "t2.micro"
}
{% endhighlight %}  
  
### Code breakdown  
