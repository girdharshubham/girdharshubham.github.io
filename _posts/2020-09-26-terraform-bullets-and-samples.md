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
    ami                     = "ami-098f16afa9edf40be"
    instance_type           = "t2.micro"
}
{% endhighlight %}  
  
### Code breakdown  
* `terraform {}` - this block is required and it's here just so Terraform knows which provider to download from the Terraform Registry. `hashicorp/aws` is a shorthand for `registry.terraform.io/hashicorp/aws`. Without the version, Terraform will download the most recent version.  
* `provider "" {}` - this block configures the providers. A provider is just another plugin that Terraform uses to translate API interactions with the service. In effect this provider will be responsible for interacting with `aws` - So, it this where the credentials go. Leave it empty because we don't want to commit sensitive information to VCS.  
* `resource "" "" {}` -  defines piece of infrastructure. A resource can be a physical component(**ec2**) or it can be a logical component(**rds**). `"" ""` that come right after `resource` specify `resourcetype` and `resourcename`. Notice in the example above, the prefix maps to the provider. Within `{}` arguments for the resource go within this block. Example: **vm size**, **vpc**, etc.  
  
## Step 4. Initialize
{% highlight shell %}
$ terraform init
Initializing the backend...

Initializing provider plugins...
- Finding hashicorp/aws versions matching "~> 3.0"...
- Installing hashicorp/aws v3.8.0...
- Installed hashicorp/aws v3.8.0 (signed by HashiCorp)

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.

{% endhighlight %}  

## Step 5. Format - Because we want our code to always look pretty!
The command below will format your code and output names of files it formatted.
{% highlight shell %}
$ terraform fmt
{% endhighlight %}  

## Step 6. Validate
{% highlight shell %}
$ terraform validate

Error: Missing required argument

  on main.tf line 10, in provider "aws":
  10: provider "aws" {}

The argument "region" is required, but no definition was found.

{% endhighlight %}  
We can ignore this error as it is benign. We're going to pass provider specific values using env vars.  

## Step 7. Export variables and create a Terraform plan  
{% highlight shell %}
$ export AWS_ACCESS_KEY_ID="anaccesskey"
$ export AWS_SECRET_ACCESS_KEY="asecretkey"
$ export AWS_DEFAULT_REGION="us-east-1"
$ terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.


------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.ami_instance will be created
  + resource "aws_instance" "ami_instance" {
      + ami                          = "ami-098f16afa9edf40be"
      + arn                          = (known after apply)
      + associate_public_ip_address  = (known after apply)
      + availability_zone            = (known after apply)
      + cpu_core_count               = (known after apply)
      + cpu_threads_per_core         = (known after apply)
      + get_password_data            = false
      + host_id                      = (known after apply)
      + id                           = (known after apply)
      + instance_state               = (known after apply)
      + instance_type                = "t2.micro"
      + ipv6_address_count           = (known after apply)
      + ipv6_addresses               = (known after apply)
      + key_name                     = (known after apply)
      + outpost_arn                  = (known after apply)
      + password_data                = (known after apply)
      + placement_group              = (known after apply)
      + primary_network_interface_id = (known after apply)
      + private_dns                  = (known after apply)
      + private_ip                   = (known after apply)
      + public_dns                   = (known after apply)
      + public_ip                    = (known after apply)
      + secondary_private_ips        = (known after apply)
      + security_groups              = (known after apply)
      + source_dest_check            = true
      + subnet_id                    = (known after apply)
      + tenancy                      = (known after apply)
      + volume_tags                  = (known after apply)
      + vpc_security_group_ids       = (known after apply)

      + ebs_block_device {
          + delete_on_termination = (known after apply)
          + device_name           = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + snapshot_id           = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }

      + ephemeral_block_device {
          + device_name  = (known after apply)
          + no_device    = (known after apply)
          + virtual_name = (known after apply)
        }

      + metadata_options {
          + http_endpoint               = (known after apply)
          + http_put_response_hop_limit = (known after apply)
          + http_tokens                 = (known after apply)
        }

      + network_interface {
          + delete_on_termination = (known after apply)
          + device_index          = (known after apply)
          + network_interface_id  = (known after apply)
        }

      + root_block_device {
          + delete_on_termination = (known after apply)
          + device_name           = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run
{% endhighlight %}  

Terraform will output a list of resources that it will create when `terraform apply` is run

## Step 8. Create Infrastructure  
**Note:** Make sure that the ami code corresponds with the region.  
{% highlight shell %}
$ terraform apply

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.ami_instance will be created
  + resource "aws_instance" "ami_instance" {
      + ami                          = "ami-098f16afa9edf40be"
      + arn                          = (known after apply)
      + associate_public_ip_address  = (known after apply)
      + availability_zone            = (known after apply)
      + cpu_core_count               = (known after apply)
      + cpu_threads_per_core         = (known after apply)
      + get_password_data            = false
      + host_id                      = (known after apply)
      + id                           = (known after apply)
      + instance_state               = (known after apply)
      + instance_type                = "t2.micro"
      + ipv6_address_count           = (known after apply)
      + ipv6_addresses               = (known after apply)
      + key_name                     = (known after apply)
      + outpost_arn                  = (known after apply)
      + password_data                = (known after apply)
      + placement_group              = (known after apply)
      + primary_network_interface_id = (known after apply)
      + private_dns                  = (known after apply)
      + private_ip                   = (known after apply)
      + public_dns                   = (known after apply)
      + public_ip                    = (known after apply)
      + secondary_private_ips        = (known after apply)
      + security_groups              = (known after apply)
      + source_dest_check            = true
      + subnet_id                    = (known after apply)
      + tenancy                      = (known after apply)
      + volume_tags                  = (known after apply)
      + vpc_security_group_ids       = (known after apply)

      + ebs_block_device {
          + delete_on_termination = (known after apply)
          + device_name           = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + snapshot_id           = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }

      + ephemeral_block_device {
          + device_name  = (known after apply)
          + no_device    = (known after apply)
          + virtual_name = (known after apply)
        }

      + metadata_options {
          + http_endpoint               = (known after apply)
          + http_put_response_hop_limit = (known after apply)
          + http_tokens                 = (known after apply)
        }

      + network_interface {
          + delete_on_termination = (known after apply)
          + device_index          = (known after apply)
          + network_interface_id  = (known after apply)
        }

      + root_block_device {
          + delete_on_termination = (known after apply)
          + device_name           = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_instance.ami_instance: Creating...
aws_instance.ami_instance: Still creating... [10s elapsed]
aws_instance.ami_instance: Still creating... [20s elapsed]
aws_instance.ami_instance: Still creating... [30s elapsed]
aws_instance.ami_instance: Still creating... [40s elapsed]
aws_instance.ami_instance: Still creating... [50s elapsed]
aws_instance.ami_instance: Creation complete after 53s [id=i-0ce0c26decc1b09f9]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
{% endhighlight %}  

## Step 9. Clean up
Run the command below to delete all resources that were created
{% highlight shell %}
$ terraform destroy
aws_instance.ami_instance: Refreshing state... [id=i-0ce0c26decc1b09f9]

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # aws_instance.ami_instance will be destroyed
  - resource "aws_instance" "ami_instance" {
      - ami                          = "ami-098f16afa9edf40be" -> null
      - arn                          = "arn:aws:ec2:us-east-1:004420258496:instance/i-0ce0c26decc1b09f9" -> null
      - associate_public_ip_address  = true -> null
      - availability_zone            = "us-east-1a" -> null
      - cpu_core_count               = 1 -> null
      - cpu_threads_per_core         = 1 -> null
      - disable_api_termination      = false -> null
      - ebs_optimized                = false -> null
      - get_password_data            = false -> null
      - hibernation                  = false -> null
      - id                           = "i-0ce0c26decc1b09f9" -> null
      - instance_state               = "running" -> null
      - instance_type                = "t2.micro" -> null
      - ipv6_address_count           = 0 -> null
      - ipv6_addresses               = [] -> null
      - monitoring                   = false -> null
      - primary_network_interface_id = "eni-00de321fa4065b50a" -> null
      - private_dns                  = "ip-172-31-21-33.ec2.internal" -> null
      - private_ip                   = "172.31.21.33" -> null
      - public_dns                   = "ec2-54-226-141-98.compute-1.amazonaws.com" -> null
      - public_ip                    = "54.226.141.98" -> null
      - secondary_private_ips        = [] -> null
      - security_groups              = [
          - "default",
        ] -> null
      - source_dest_check            = true -> null
      - subnet_id                    = "subnet-09d3a244" -> null
      - tags                         = {} -> null
      - tenancy                      = "default" -> null
      - volume_tags                  = {} -> null
      - vpc_security_group_ids       = [
          - "sg-d67708ec",
        ] -> null

      - credit_specification {
          - cpu_credits = "standard" -> null
        }

      - metadata_options {
          - http_endpoint               = "enabled" -> null
          - http_put_response_hop_limit = 1 -> null
          - http_tokens                 = "optional" -> null
        }

      - root_block_device {
          - delete_on_termination = true -> null
          - device_name           = "/dev/sda1" -> null
          - encrypted             = false -> null
          - iops                  = 100 -> null
          - volume_id             = "vol-0277d8dbfaa2add75" -> null
          - volume_size           = 10 -> null
          - volume_type           = "gp2" -> null
        }
    }

Plan: 0 to add, 0 to change, 1 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

aws_instance.ami_instance: Destroying... [id=i-0ce0c26decc1b09f9]
aws_instance.ami_instance: Still destroying... [id=i-0ce0c26decc1b09f9, 10s elapsed]
aws_instance.ami_instance: Still destroying... [id=i-0ce0c26decc1b09f9, 20s elapsed]
aws_instance.ami_instance: Destruction complete after 26s

Destroy complete! Resources: 1 destroyed.

{% endhighlight %}  

