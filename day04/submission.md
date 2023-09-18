# TerraWeek Day 4- State File Management🌱

Welcome to **Day 4** of Terraweek!, we'll dive deeper into understanding **Terraform state files, their importance, and how to manage them effectively.**

## Table of Contents
- [Terraform State File (terraform.tfstate)](#terraform-state-file-terraformtfstate)
  - [What is Terraform State (terraform.tfstate) file?](#1-what-is-terraform-state-terraformtfstate-file)
  - [How Does Terraform State Store Information?](#2-how-does-terraform-state-store-information)
  - [Managing Terraform State](#3-managing-terraform-state)
- [Purpose of Terraform State File](#purpose-of-terraform-state-file)
- [Terraform State File Metadata](#terraform-state-file-metadata)
- [Terraform State Performance and Caching](#terraform-state-performance-and-caching)
- [terraform_remote_state Data Source](#terraform_remote_state-data-source)
- [Terraform State Storage, Manual State Pull/Push, and Locking](#terraform-state-storage-manual-state-pullpush-and-locking)
  - [Storage](#81-storage)
  - [Manual State Pull/Push](#82-manual-state-pullpush)
  - [Terraform State Locking](#83-terraform-state-locking)
- [Conclusion](#conclusion)


## Terraform State File (terraform.tfstate)

Terraform state is at the core of Terraform's infrastructure management. It's a critical component that helps in tracking, maintaining, and ensuring the integrity of your infrastructure setup. 

### 1. What is Terraform State (terraform.tfstate) file?

The `terraform.tfstate` file acts as a recorder for your infrastructure setup. It's created from the very beginning when you run your first `terraform apply` command. This file stores critical information about your infrastructure, including the current state of resources. Any change you do with your infrastructure will have its presence in the terraform.tfstate file. So when you work with Terraform for managing and provisioning your infrastructure then terraform will always create a terraform.tfstate file for you.

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/e521eed0-32be-4710-907f-cd5d39f30749)


### 2. How Does Terraform State Store Information?

Terraform state file captures metadata about your infrastructure resources. Any changes you make or plan to make are stored inside this file. It's essential to note that you don't need to manage the information within the Terraform state file manually. Terraform takes care of updating it automatically during `terraform plan` and `terraform apply` operations.

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/439b1274-a39d-4985-a05c-7be7080f8079)


### 3. Managing Terraform State

1. **Saving Locally:** If you're the sole developer managing your infrastructure with Terraform, the state file is generated and saved locally by default. The `terraform apply` command generates the state file in your working directory.

2. **Saving Remotely:** In a collaborative team environment where multiple developers work on Terraform, it's highly recommended to store the state file remotely, such as in an AWS S3 bucket. This ensures that your state file is up-to-date and synchronized with your team's changes.

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/ac3d0e42-f797-4465-ba2f-9673b5ffe9a8)


## What is the Purpose of Terraform State File?

The Terraform state file plays a crucial role in the infrastructure management process:

- **Recording Changes:** Any changes made to your resources are first reflected in the Terraform state file before updating the cloud infrastructure. It captures every action taken on Terraform resources.

- **Validation:** Before executing `terraform apply` or `terraform destroy`, Terraform validates your resource configuration with the state file. It ensures that the changes are valid and then updates the state file and the actual resource.

Now the question comes how can terraform differentiate the multiple resources of the same type running in the cloud environment?

- **Resource Differentiation:** Terraform has its own local database known as Terraform state file where each resource is tagged with its own unique id and this unique id is stored in the state file in the form of metadata.

## Terraform State File Metadata

Metadata in the Terraform state file represents the information about your infrastructure setup. It includes details about resources, their attributes, and dependencies. Terraform maintains order and structure within the state file for efficient management.

**Let's take an example -**

Step-1: You have written a resource block resource "aws_instance" "ec2_example"
Step-2: Terraform has generated a terraform.state file to store metadata about ec2_instance
Step-3: You have added an S3 bucket manually from AWS Console without making any changes to terraform file.
Step-4: Now you have run the terraform destroy command.
End Result: Terraform only delete EC2 instance
Conclusion - It can only find EC2 instance resource metadata into terraform state file and there is no reference of S3 bucket because you have created the bucket manually and terraform state file have no clue of S3 Bucket.

This is how Terraform maintains a dependency order for maintaining the infrastructure inventory. It is highly recommended to always have provision or update your infrastructure only with the terraform.

Here is a sample copy of metadata of my terraform state file -

## Terraform State Performance and Caching

The next benefit of Terraform state is file Caching. As terraform, state file acts as a local database for your terraform code, so before making any request to cloud infrastructure it first checks the local terraform state file to show the current status of the cloud infrastructure

Imagine you have a really large infrastructure and if you wanna make a query to get information status about each resource running in cloud infrastructure. It could take really long and would not be a very effective way to know the state of your cloud infrastructure.

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/168828ca-56e2-4db5-9526-dbf21073ae91)

Terraform state provides caching capabilities, making it efficient for querying the state of your infrastructure. Terraform plan is a prime example of state caching. When you run `terraform plan`, it returns information about:

- Resources to be added
- Resources to be changed
- Resources to be destroyed

This information is retrieved from the state file, offering performance benefits.

## What is `terraform_remote_state` Data Source?

In a collaborative environment with multiple developers, maintaining a single Terraform state file is essential. To retrieve metadata from the remote state file, you can use the `terraform_remote_state` data source.

Let's assume you are a team of multiple developers working on the same Terraform project to set up and maintain your cloud infrastructure.

Considering the team size all the developers will maintain a single terraform state file so that every change which they make is always in sync with other teammates.

But is there a way to retrieve the metadata information from the remote state file?

Well yes, you can use terraform_remote_state data source to retrieve the metadata information of the remote state file.

Remote backend -

Here is an example code snippet of terraform_remote_state with remote backend -

**Remote Backend Example:**

```hcl
data "terraform_remote_state" "remote_state" {
  backend = "remote"

  config = {
    organization = "jhooq"
    workspaces = {
      name = "jhooq-test"
    }
  }
}

# Terraform >= 0.12
resource "aws_instance" "jhooq-test" {
  subnet_id = data.terraform_remote_state.remote_state.outputs.subnet_id
}
```

**Local Backend -**

```hcl
data "terraform_remote_state" "local_state" {
  backend = "local"

  config = {
    path = "/home/rwagh/terraform/terraform.tfstate"
  }
}

# Terraform >= 0.12
resource "aws_instance" "jhooq-test" {
  # ...
  subnet_id = data.terraform_remote_state.local_state.outputs.subnet_id
}
```

## Terraform State Storage, Manual state pull/push and Locking

1. **Storage**
Terraform state files is can be stored on local as well as on remote it depends on your need. Storage of terraform state file is defined by the backend.

**Local Storage**- Consider the following example of where terraform state storage is local -

```
data "terraform_remote_state" "remote_state" {
  backend = "local"
}
```
- When using the local storage terraform will persist/save the state file on the local disk
- But if you are using the local storage then you have to manually push and pull the Terraform state file.

  **Remote Storage-** Here is one more example in which we have defined the storage as remote -
  ```hcl
  data "terraform_remote_state" "remote_state" {
  backend = "remote"
}


- When using the remote storage terraform will not persist/save the state file on the local disk.
- Remote storage of Terraform state files is really beneficial for teams with multiple developers

2. **Manual state pull/push**

In the previous point, we discussed how to store state files locally and remotely. But apart from storing the state file there two more important operations -

Pull terraform state file
Push terraform state file
1. Pull terraform state file- It is quite often for a developer to take a break from work and during the break if developer's terraform project is out of sync then it is always recommended to get in sync with all the updates which has happened.

So apart from doing the normal git pull it is also good to do the terraform state pull also so that all the latest changes which have been done on to terraform state file will be retrieved -
```
terraform state pull 
```

2. Push terraform state file - It is a dangerous command to work with. It can potentially lead you to an inconsistent state where you might override other team member changes on to terraform state file. It is often discouraged to use terraform state push command.
```
terraform state push
```

## How to terraform prevents us from accidental state push?

1. Differing Lineage- Terraform has a safety mechanism on to terraform state file. Each time you create a terraform state file it will assign a lineage ID to the state file. If you are attempting to push the terraform state file with a different lineage ID then terraform will not allow it.

2. Higher Serial Number- Apart from the unique lineage ID terraform also assigns a unique and higher serial number to terraform state file. If you are attempting to push terraform state file which has a lower higher serial number than Terraform will not allow you to push terraform state file.

How to force push to terraform state file?

Considering all the safety mechanisms (lineage, Higher serial number) provided by Terraform, if you still wanna override those safety settings then you can use -force flag to push your Terraform state file.
```
terraform state push -force
```

3. Terraform State Locking
Terraform has one more safety mechanism known as locking by default terraform puts a lock on the Terraform state file(if the backend supports the locking).

Terraform state locking will prevent any other developer from updating terraform state file. Terraform locking always happens behind the scene but if terraform locking fails then terraform will not let you continue.

# Conclusion
Terraform state file is always the core heart of terraform project. You must treat the terraform state file with care otherwise, you might end up in a situation where your terraform project is full of mess and hard to manage.

The best practices for using terraform state file which I could recommend is -

If you have more than one developer working on Terraform project then it is always recommended to use remote backend to store your state file remotely.
If you are storing terraform state file remotely then you should always use terraform state pull before you start working with your terraform project
For sensitive data always consider securing the terraform state file because by default terraform store the state file in the JSON format.
Do not try to force push the terraform state file, it will always lead you to inconsistent behavior.



