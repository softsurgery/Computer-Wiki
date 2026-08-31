As modern applications become more complex, companies need reliable ways to create and manage their infrastructure. Traditionally, engineers had to manually create servers, networks, databases, and other resources through cloud provider dashboards. This approach can become difficult to maintain, especially when an organization has multiple environments such as development, testing, staging, and production.

**Terraform** is a tool that helps solve this problem by allowing infrastructure to be defined and managed using code.

Terraform follows the concept of **Infrastructure as Code (IaC)**. Instead of manually configuring infrastructure, engineers describe the desired infrastructure in configuration files, and Terraform creates or modifies the required resources automatically.

---
# What Is Terraform?

**Terraform is an Infrastructure as Code (IaC) tool developed by HashiCorp.**
It allows you to define, provision, and manage infrastructure using configuration files written primarily in **HashiCorp Configuration Language (HCL)**.
For example, instead of manually creating a virtual machine on a cloud platform, you can describe it in a Terraform configuration:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t3.micro"
}
```

Terraform reads this configuration and determines what infrastructure needs to be created.
Terraform can manage resources from many providers, including cloud platforms such as AWS, Microsoft Azure, and Google Cloud, as well as Kubernetes and many other services.

---

# Why Do We Need Terraform?

Without Infrastructure as Code, infrastructure is often created manually.

For example:
1. Log in to a cloud provider.
2. Create a network.
3. Create a server.
4. Configure security rules.
5. Create a database.
6. Configure storage.
7. Repeat the process for another environment.

This can lead to:

- Human errors
- Configuration differences between environments
- Difficulties reproducing infrastructure
- Poor documentation
- Time-consuming deployments

Terraform allows the infrastructure configuration to be stored as code.

That means the configuration can be:
- Version controlled with Git
- Reviewed by other engineers
- Reused
- Automated
- Reproduced across environments

---
# How Terraform Works

Terraform works by comparing the **desired state** defined in your configuration with the **current state** of your infrastructure.
For example, suppose your Terraform configuration says:

```text
2 web servers
1 database
1 network
```

Terraform checks what currently exists and determines what changes are necessary to reach the desired state.

The general workflow is:

```text
Terraform Configuration
        ↓
terraform plan
        ↓
Review Changes
        ↓
terraform apply
        ↓
Infrastructure
```
