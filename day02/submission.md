# Day 2 of #TerraWeek — Terraform Configuration Language (HCL)

Hello connections, It’s my day 2 of the 7 days Terraweek challenge, where we will dive into the basic concepts of Terraform, a powerful configuration language called HCL (HashiCorp Configuration Language).

This is day 2 of the #TerraWeek challenge led by Shubham Londhe. In this blog, we’ll explore about:

- ✨ HCL blocks, parameters, and arguments
- ✨ Variables, Data Types, and Expressions
- ✨ Writing Terraform Configurations
- ✨ Testing and Debugging the configuration

## HCL blocks, parameters, and arguments

### HCL Syntax:

HCL is a **configuration language used by Terraform to define infrastructure as code.** It is a simple, human-readable language that allows you to define resources and data sources in a declarative way.

#### Blocks:

In HCL, everything revolves around blocks. **Blocks are used to define resources, variables, data sources, and more.** They provide a way to organize and structure your code. A block is identified by its type and is defined using curly braces {}.

Let’s take a simple example of a block that defines an AWS EC2 instance:

```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```
In this example, resource is the block type, and aws_instance is the label for the block. The block's contents, enclosed within curly braces, specify the properties of the EC2 instance, such as the Amazon Machine Image (AMI) and instance type.

#### Parameters: Variables within Blocks

Parameters are variables within blocks that allow you to define reusable values. They enable you to create dynamic and flexible configurations. You can think of them as placeholders that can be assigned different values when using the block.

Continuing with our previous example, let’s introduce parameters:

```hcl
resource "aws_instance" "example" {
  ami           = var.ami_id
  instance_type = var.instance_type
}
```

In this updated code snippet, var.ami_id and var.instance_type are parameters. Instead of hard-coding the values directly, we refer to the variables ami_id and instance_type. This way, we can pass different values to these parameters when using the block, making our code more reusable.

#### Arguments: Assigning Values to Parameters
Now that we have parameters, we need a way to assign values to them. This is where arguments come into play. Arguments are used to provide specific values to parameters within a block.

Let’s see an example of assigning arguments to our previous block:

```hcl
module "example" {
  source = "./modules/ec2"

  ami_id        = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```
In this example, we are using the module block to instantiate an EC2 instance from a reusable module located in the ./modules/ec2 directory. We assign values to the parameters ami_id and instance_type using the equals sign =.

Notice how the parameter names (ami_id and instance_type) match the variable names used within the block definition. This is important for the proper assignment of values.

#### Variables, Data Types, and Expressions
Variables are an important part of Terraform configurations. They allow you to define values that can be reused throughout your configuration. In HCL, variables are defined using the variable block.

Defining Variables:
To define a variable in HCL, create a variables.tf file and add the following code:

```hcl
variable "server_count" {
  type    = number
  default = 3
}
```
In this example, the server_count variable is defined with a type of number and a default value of "3".

Using Variables:
To utilize a variable in your Terraform configuration, you can reference it using the ${var.variable_name} syntax. Here's an example demonstrating the use of the server_count variable in a main.tf file:
```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  count         = var.server_count
}
```
In this instance, we create multiple AWS EC2 instances using the aws_instance resource. The count attribute is set to var.server_count, which allows us to create a specified number of instances based on the value assigned to the server_count variable. In this case, it will create three EC2 instances.

#### Data Types and Expressions:
HCL supports various data types, including strings, numbers, booleans, lists, and maps. Expressions allow you to manipulate these data types. Let’s explore an example that demonstrates data types and expressions in Terraform:

```hcl
variable "age" {
  type    = number
  default = 30
}

variable "name" {
  type    = string
  default = "John Doe"
}

variable "is_adult" {
  type    = bool
  default = var.age >= 18
}

output "greeting" {
  value = "Hello, ${var.name}! You are ${var.age} years old."
}

output "adult_status" {
  value = var.is_adult ? "You are an adult." : "You are not an adult."
}
```
In this example, we have three variables: age, name, and is_adult. The age variable is of type number with a default value of 30. The name variable is of type string with a default value of "John Doe". The is_adult variable is of type bool, and its value is determined based on whether the age variable is greater than or equal to 18.

The first output block uses string interpolation to create a greeting that includes the value of the name and age variables. The second output block uses a conditional expression to display a message depending on the value of the is_adult variable.

When you run Terraform with this configuration, it will provision the desired number of EC2 instances based on the server_count variable and display the outputs with the interpolated values.

### Writing Terraform Configurations
Now that we have covered the basics of HCL syntax, variables, data types, and expressions, let’s write a Terraform configuration using HCL syntax.

#### Adding Required Providers
Docker Provider:
The Docker provider allows you to manage Docker containers and related resources. To add the Docker provider to your Terraform configuration, you need to include the required_providers block and specify the particular version.

```hcl
terraform {
  required_providers {
    docker = {
      source = "kreuzwerker/docker"
      version = "2.11.0"
    }
  }
}

```

AWS Provider:
The AWS provider is one of the most commonly used providers in Terraform. It allows you to provision and manage resources in Amazon Web Services (AWS).

```hcl
provider "aws" {
  region = "us-east-1"
}
```

Creating Resources
Next, we can create resources using HCL syntax. Here is an example of how to create a Docker container:

```hcl
resource "docker_container" "example" {
  name  = "nginx-container"
  image = "nginx:latest"
  ports {
    internal = 80
    external = 8080
  }
}
```

In this example, we are creating a Docker container called nginx-container with the latest version of the nginx image and mapping port 80 to port 8080.

Creating Data Sources
Data sources allow you to retrieve information about existing resources. Here is an example of how to create a data source that retrieves information about an AWS EC2 instance:

```hcl
data "aws_instance" "example" {
  instance_id = "i-0123456789abcdef0"
}
```
In this example, we are creating a data source that retrieves information about an AWS EC2 instance with the ID i-0123456789abcdef0.

# Testing and Debugging

Testing and debugging your Terraform configurations is an important part of the development process.

Here are some tips and best practices for testing and debugging your configurations:

- Use the `terraform plan` command to preview changes before applying them.
- Use the `terraform apply` command to apply changes to your infrastructure.
- Use the `terraform destroy` command to destroy resources created by your configuration.
- Use the `terraform state` command to view the current state of your infrastructure.
- Use the `terraform validate` command to check your configuration for syntax errors.
- Use the `terraform fmt` command to format your configuration for consistency.

# Conclusion

Terraform is your magic wand for creating and managing infrastructure using code. In this blog, we’ve covered HCL blocks, parameters, and arguments, explored different resource types, and written configurations for providers like Docker and AWS. We also emphasized the importance of testing and debugging for reliable infrastructure. With Terraform, you can enjoy reproducibility, scalability, and maintainability in your projects. Armed with this knowledge, you can now confidently start crafting your infrastructure blueprints using Terraform. So, go ahead and unleash the power of Terraform to transform your infrastructure with ease!

Remember, practice makes perfect, and the Terraform documentation is your trusty guide. Happy Terraforming!

#terraform #aws #DevOps #TrainWithShubham #TerraWeekChallenge #90daysofdevops #happylearning #AWS #IAC #TerraWeek


