# **Terraweek Day 3: Defining and Managing Resources with Terraform**

Welcome to **Day 3** of Terraweek! Today, we'll dive deeper into the world of Terraform and focus on understanding how to **define and manage resources**. We'll also explore various resource types and their configurations. Additionally, we'll touch on **resource dependencies**, ***provisioners**, and **lifecycle management**. By the end of this guide, you'll have a solid grasp of these crucial concepts in Terraform. Let's get started!

## Table of Contents
- [Defining and Managing Resources with Terraform](#defining-and-managing-resources-with-terraform)
- [Check State Files and Validate Your .tf File](#check-state-files-and-validate-your-tf-file)
- [Resource Dependencies & Provisioners](#resource-dependencies--provisioners)
- [Lifecycle Management](#lifecycle-management)
- [Conclusion](#conclusion)


## **Defining and Managing Resources with Terraform**
Create a Terraform configuration file to define a resource of AWS EC2 instance:

**1. Create a *Ubuntu Machine* on AWS**
   
**2. Install Terraform:**
Download the Terraform binary suitable for your operating system from the [official Terraform website](https://www.terraform.io/downloads.html).
In this demo, I’m setting up Terraform in an AWS EC2 instance(Ubuntu AMI):

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/5a5c0cef-bcd9-453d-a631-0edf744b2f61)

Verify the installation using **terraform — version:**

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/4dcee0d1-dec5-4795-b138-f94bc9934479)

**3. Set Up AWS Credentials:**
- Sign in to the AWS Management Console.
- Create an IAM user or use an existing one with appropriate permissions for managing AWS resources. Ensure the user has permissions for EC2, VPC, S3, and other services you plan
  to work with.
  
![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/b8356fab-0ea5-4406-9ecf-b31b36d44fe0)

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/72fc48ab-c854-4a8f-84ba-029f2f038b7f)

**choose CLI or any reason as per your usage**
![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/6911947a-ba85-4485-ab84-a4e8e4512143)

**provide name to your credentials**
![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/0e74e527-5c49-458a-aa06-a544e28c1dfc)

**Download .csv file & keep it safe. It won’t be recovered later.**
![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/b8bd5f8c-eaa5-4fad-8684-0c3ea52891f9)

- Generate an **access key and secret key** for the IAM user. Make a note of these credentials as they will be required for Terraform to interact with AWS.
- Configure the **AWS CLI** on your EC2 machine by running the following command in your terminal:
 
![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/c9a650d0-d3d3-4fb3-9dc9-ec24529a0013)

**4. Create Terraform configuration file:**

- Create a new directory for your Terraform configuration files.
- Open a terminal and navigate to the new directory.
- Create a new Terraform configuration file with an .tf extension (e.g., main. tf). This file will contain your infrastructure code.
- In the configuration file, specify the AWS provider and any resources you want to create or manage. For example, you can add the following code to create an AWS EC2 instance:

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/6121c985-7e66-40e3-bfa4-582fa55354af)

### **Check State Files and Validate Your .tf File**

Check state files before running plan and apply commands & Use validate command to validate your .tf file for errors and provide the Output generated by each commands.

1. Initialize the working directory using the init command:
**terraform init**

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/7be3dc1b-48dc-4fad-8870-ef51d394fd62)

3. Check the existing state files to ensure that Terraform is aware of the current infrastructure:
**terraform state list**

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/f89cb874-0e9f-4ed3-a8f8-a6c66061128c)

5. If your Terraform configuration files have any formatting issues, the terraform fmt command will automatically fix them.
   **terraform fmt**
6. Run the following command to validate the configuration file for syntax errors and other issues:
**terraform validate**

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/dd0ac000-9310-4865-876f-12342dbe4fd1)

8. Once you’ve verified the state and validated the configuration file, you can proceed with the plan and apply commands.
**terraform plan
terraform apply**

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/ac655c48-08ab-48eb-8cc7-10a8335bc840)

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/e542a466-d06c-4e4f-82bb-d2843dd8eedd)

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/0fbfb3e4-489a-4994-a4a5-23a88180a2b6)

**Boom, our EC2 instance is up and running!**

6. **terraform destroy** command removes all the resources defined in the Terraform configuration, effectively destroying the infrastructure.
![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/fcaf7b05-0f73-4443-b6be-404a13e1a0ae)

### **Resource Dependencies & Provisioners**

- **Resource Dependencies:**
Resource dependencies are like the **connections between different parts of your infrastructure.** They tell Terraform **the order in which things should be created or changed.** When you set up resource dependencies, you’re making sure that Terraform knows how to build your infrastructure in the right order, considering how different parts depend on each other.

Let’s say you have an application that needs a web server and a database. You want to make sure the web server is set up first before the database because the database relies on the web server. By specifying the dependency, you’re telling Terraform that the database resource should wait until the web server is ready before it gets created or modified.

To set up a dependency, you can use the **depends_on** attribute in your resource block. You put the resources that depend on others inside this attribute. For example:

![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/51508b5a-c426-4e96-9612-2ea416df01c6)

In this case, we have **two resources: the aws_instance and the aws_db_instance.** We want the aws_db_instance to wait until the aws_instance is ready. So we put **aws_instance.my-example inside the depends_on attribute of the aws_db_instance.database resource.**

When you run terraform apply, Terraform will figure out the right order based on these dependencies. It will make sure that everything gets set up correctly, taking into account the relationships between the resources.

- **Provisioners:**
Provisioners in Terraform are used **to execute scripts or other actions on a local or remote machine as part of the resource creation or destruction process.** Here’s an example of using a remote-exec provisioner to run a script on an AWS EC2 instance:
![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/cd7d200b-a79f-4e7a-b93c-6254abfc3942)

### **Lifecycle Management**
Terraform allows you to manage the lifecycle of resources using the lifecycle block. This block can be used to configure how Terraform should **handle resource creation, updates, and destruction.**

- ***create_before_destroy**: This setting controls whether to create a new resource before destroying the old one during an update. By default, Terraform destroys the old resource first and then creates the new one. Enabling this setting allows for zero-downtime replacements.
  
- ***prevent_destroy:** Setting prevent_destroy to true prevents a resource from being destroyed. This can be useful to protect critical resources from accidental deletion.
  
- ***ignore_changes:** ignore_changes allows you to specify resource attributes that Terraform should ignore when determining if a resource needs to be replaced. This can be helpful when certain attributes are managed outside of Terraform.
   
Here’s an example of using the lifecycle block to prevent the accidental destruction of an AWS EC2 instance:
![image](https://github.com/sakshirathoree/TerraWeek/assets/67737704/1e06f255-e181-415c-8658-7f92af834653)

### **Conclusion:**
In this blog post, we’ve explored how to **manage resources using Terraform, focusing on various resource types and their configurations**, such as AWS EC2 instances. We’ve also discussed **resource dependencies, provisioners, and lifecycle management**. Armed with this knowledge, we can effectively provision and manage our infrastructure using Terraform’s powerful capabilities. Happy provisioning!

The above information is up to my understanding. Suggestions are always welcome.

Thank you for reading! Connect me on LinkedIn: [**Rajlaxmi Rathore**](https://www.linkedin.com/in/rajlaxmi-rathore-97880a1b2/)
