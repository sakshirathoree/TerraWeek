# 7 Days Terraweek Challenge: Getting Started with Terraform

Hello connections, It’s my day 1 of the 7 days Terraweek challenge, where we will dive into the basic concepts of Terraform.

-  What is Terraform and how does it revolutionize infrastructure management?
-  Why do we need it and how does it simplify provisioning?
-  Learn crucial Terraform terminologies with examples.
-  Understand Terraform Lifecycle.
-  Easily install Terraform and configure environments for AWS.

Before we deep dive into Terraform, let’s first understand the **Infrastructure as Code (IaC).**

### What Is Infrastructure as Code (IaC)?

Infrastructure as Code (IaC) is a fancy term that means managing your infrastructure using code. But what does that even mean? Think of it as a recipe for building things, but instead of cooking, we’re talking about technical stuff like servers, networks, and databases.

Using IaC, you write instructions in a special language that tells your computer how to create and manage your infrastructure. It’s like giving your computer a super detailed to-do list, and it follows those instructions to make your infrastructure dreams come true.

There are various IaC tools available like Terraform, Chef, Puppet, and Ansible. Read the blog to know why Terraform is preferred over other IaC tools.

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/81524319-ace8-4521-ad33-6735e44f7c3f)


Let’s now understand What Is Terraform?

## What Is Terraform?

Terraform is like a magic wand that lets you **create and manage your infrastructure using code.** But what does that mean? Well, instead of clicking buttons or typing commands, you write simple instructions in a special language that Terraform understands. With Terraform, you can describe your desired infrastructure state in configuration files, similar to writing a recipe, and Terraform will work its magic to create and maintain your infrastructure.

Terraform is one of the most popular open-source Infrastructure-as-code (IaC) tools, used to automate infrastructure tasks. It is used to automate the provisioning of your cloud resources.

### Why is Code Cool for Infrastructure?

You might wonder why we need to use code for infrastructure. Good question! Think of it this way: When you write code, you can easily share it, keep track of changes, and work together with others. With Terraform, you can do the same thing with your infrastructure. You have a set of instructions (your code) that you can use again and again to create awesome things (your infrastructure).

### Why do we need Terraform and How Does it Simplify Infrastructure Provisioning?

Managing infrastructure manually is prone to human errors, is time-consuming, and lacks consistency. This is where Terraform comes to the rescue! Here’s why we need Terraform and how it simplifies infrastructure provisioning:

- **Declarative Configuration:** Terraform uses simple and readable language to describe your infrastructure requirements. Instead of worrying about the specific steps to create each resource, you can focus on what you want your infrastructure to look like. It’s like telling Terraform your wish, and it grants it to you!

- **Automation Made Easy:** With Terraform, you can automate the provisioning and management of your infrastructure. That means you can sit back and relax while Terraform does the heavy lifting for you. It’s like having a robot friend who follows your instructions flawlessly.

- **Multi-Cloud Magic:** Want to use multiple cloud providers like AWS, Azure, or Google Cloud? No problem! Terraform supports various cloud platforms, making it a breeze to manage resources across different providers. It’s like having a universal remote control for your infrastructure.

- **Infrastructure as Code:** With Terraform, you can treat your infrastructure as code, just like you would with a cool video game. You write your infrastructure specifications in code, which brings a bunch of benefits. You can version control your code, collaborate with others, and reuse code snippets.

- **Plan and Apply:** Terraform helps you plan ahead before making any changes to your infrastructure. It analyzes your code and shows you a preview of what will happen when you apply it. This way, you can review and double-check everything, ensuring a smooth and error-free provisioning process. It’s like having a virtual assistant who shows you a blueprint before building your dream house.

- **State Management:** Terraform keeps track of the current state of your infrastructure in a file. This state file is like a map that Terraform uses to understand what resources exist and their configurations. It helps Terraform make intelligent decisions when you modify or destroy resources. It’s like having a memory bank for your infrastructure changes.

### Common Terminologies in Terraform

- **Provider:** Imagine you want to build a house, but you need land to do so. In Terraform, a provider is like a landowner who gives you the resources to build your infrastructure. Providers are responsible for managing various cloud platforms or infrastructure providers, such as AWS, Azure, or Google Cloud. Terraform officially supports around 130 providers.
 Here’s an example configuration block for the AWS provider:

    ```
    provider "aws" {
      region = "us-west-2"
    }
    ```

    A provider is responsible for understanding API interactions and exposing resources. It interacts with the various APIs required to create, update, and delete various resources. Terraform configurations must declare which providers they require so that Terraform can install and use them.

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/c2fd34de-16f3-4a09-8127-8ec67fa18eb4)


- **Resource:** Resources are the building blocks of your infrastructure. They represent the individual components you want to create or manage, such as servers, databases, or networks. Resources are defined within a Terraform configuration file and are associated with a specific provider. Example:

    ```terraform
    resource "aws_instance" "web_server" {
      ami           = "ami-0c55b159cbfafe1f0"
      instance_type = "t2.micro"
    }
    ```

- **Variable:** Variables are like placeholders that allow you to pass dynamic values to your Terraform code. They provide flexibility by allowing you to change values without modifying the code itself. Variables can be defined within your Terraform configuration files or in separate variable files. Example:

    ```terraform
    variable "region" {
      default = "us-west-2"
    }
    ```

- **Module:** Think of a module as a ready-to-use blueprint for a specific set of infrastructure resources. It’s like a pre-built package that you can reuse across different projects. Modules help keep your code organized and promote consistency in your infrastructure. Example:

    ```terraform
    module "vpc" {
      source = "terraform-aws-modules/vpc/aws"
      version = "2.0.0"
    
      # Additional configurations specific to your project
      vpc_name = "my-vpc"
      cidr_block = "10.0.0.0/16"
    }
    ```
*In this example, we’re using a pre-built module from the Terraform Registry to create a Virtual Private Cloud (VPC). We specify the module source, and version, and provide additional configurations specific to our project.*

- **State:** The state is like a memory bank for your infrastructure. It keeps track of the current state of your resources and their configurations. Terraform uses the state to understand what’s already created, making it easier to manage and update your infrastructure. Example:

    ```terraform
    terraform {
      backend "s3" {
        bucket = "my-terraform-state"
      }
    }
    ```

## Terraform Lifecycle

Terraform lifecycle consists of — **init, plan, apply, and destroy.**

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/52e78839-3da7-4588-ae9a-3bc581a66c16)

- **Terraform init:** initializes the (local) Terraform environment. Usually executed only once per session.

- **Terraform plan:** compares the Terraform state with the as-is state in the cloud, builds and displays an execution plan. This does not change the deployment (read-only).

- **Terraform apply:** executes the plan. This potentially changes the deployment.

- **Terraform destroy:** deletes all resources that are governed by this specific Terraform environment.

## Steps to install Terraform and set up the environment for AWS

To install Terraform and set up the environment for AWS, you can follow these steps:

1. **Install Terraform:**
   - Download the Terraform binary suitable for your operating system from the official Terraform website (https://www.terraform.io/downloads.html).
   - Verify the installation using `terraform --version`.

**In this demo, I’m setting up Terraform in an AWS EC2 instance(Ubuntu AMI):**
![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/5aa2400b-fbc5-4bbc-aa31-7dae5fbe89e0)

Verify the installation using terraform — version:
![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/a4c5ce4c-ca3e-464b-b0a8-7fafc0045d85)


2. **Set Up AWS Credentials:**
   - Sign in to the AWS Management Console.
   - Create an IAM user or use an existing one with appropriate permissions for managing AWS resources. Ensure the user has permissions for EC2, VPC, S3, and other services you plan to work with.
   - Generate an access key and secret key for the IAM user. Make a note of these credentials as they will be required for Terraform to interact with AWS.
   - Configure the AWS CLI on your EC2 machine by running the following command in your terminal:

  ```shell
    aws configure
  ```
![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/922c2a8e-15cb-4359-a2ab-0822d3ba200a)


3. **Create Terraform configuration file:**
   - Create a new directory for your Terraform configuration files.
   - Open a terminal and navigate to the new directory.
   - Create a new Terraform configuration file with a .tf extension (e.g., main.tf). This file will contain your infrastructure code.
   - In the configuration file, specify the AWS provider and any resources you want to create or manage.

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/b2acf28a-3623-43cc-b612-ee7100e20a70)


4. **Initialize Terraform:**
   - In the terminal, navigate to your Terraform directory (where your configuration file is located).
   - Run the following command to initialize Terraform and download the necessary provider plugins:

 ```shell
    terraform init
 ```
![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/a1686899-d349-4779-ae51-83e94ec3de43)


5. **Deploy Infrastructure:**
   - After initialization, run the following command to preview the changes Terraform will make to your AWS environment:

```shell
    terraform plan
```
![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/1b91a551-0152-412f-898e-5c1880580a94)


   - Review the output and ensure it matches your intentions.
   - If everything looks good, execute the following command to apply the changes and create the infrastructure:

 ```shell
    terraform apply
```
![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/82f19131-a765-45ec-887a-e2041012adcb)

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/5f979b3c-f781-489d-9da4-b747f3a6ff41)

**Boom, our EC2 instance is up and running!**

## Conclusion

Terraform is the ultimate beginner’s best friend for infrastructure provisioning. It simplifies the process by allowing you to manage infrastructure with simple code, automate tasks, and work seamlessly across multiple cloud platforms. With Terraform, you can embrace the power of declarative configuration, treat your infrastructure as code, and leverage state management for smooth operations. So, take a deep breath, embrace the magic of Terraform, and say goodbye to a tedious manual infrastructure setup. Happy provisioning!

The above information is up to my understanding. Suggestions are always welcome.

Thank you for reading! Connect me on LinkedIn: [Rajlaxmi Rathore](https://www.linkedin.com/in/rajlaxmi-rathore-97880a1b2/)
