# Launch-EC2-Instance-Using-Terraform
Launching EC2 Instance

📘 Terraform AWS EC2 Instance Documentation
# -------------------------------
# AWS EC2 Instance Configuration
# -------------------------------

# This resource block defines an EC2 instance named "web".
# The instance will use a specific Amazon Machine Image (AMI), instance type,
# key pair, and security group as defined below.

resource "aws_instance" "web" {
  
  # -------------------------------
  # AMI Configuration
  # -------------------------------
  # The AMI (Amazon Machine Image) ID is being fetched dynamically from
  # a data source named `data.aws_ami.amiID`. This allows flexibility
  # in selecting the most recent or appropriate AMI without hardcoding.
  ami = data.aws_ami.amiID.id

  # -------------------------------
  # Instance Type
  # -------------------------------
  # Defines the size and resources of the EC2 instance.
  # "t3.small" provides 2 vCPUs and 2 GB of memory, suitable for light workloads.
  instance_type = "t3.small"

  # -------------------------------
  # SSH Key Pair
  # -------------------------------
  # This key pair allows SSH access to the instance.
  # The key must exist in the AWS region before deployment.
  key_name = "dove-key"

  # -------------------------------
  # Security Group
  # -------------------------------
  # Associates the instance with one or more VPC security groups.
  # The security group controls inbound and outbound traffic rules.
  vpc_security_group_ids = [aws_security_group.dove-sg.id]

  # -------------------------------
  # Availability Zone (Optional)
  # -------------------------------
  # Uncomment and specify if you want to launch the instance in a specific AZ.
  # By default, AWS will automatically choose one within the region.
  # availability_zone = "us-east-1a"

  # -------------------------------
  # Tags
  # -------------------------------
  # Tags are key-value pairs used for identification, organization, and billing.
  tags = {
    Name    = "Dove-Web"  # The instance’s visible name in the AWS console.
    Project = "Dove"      # Project identifier for grouping resources.
  }
}

🧩 Explanation of Key Components
Element	Description
resource "aws_instance" "web"	Defines a new EC2 instance resource named web.
ami	References a pre-defined data source for selecting the correct AMI dynamically.
instance_type	Specifies the EC2 instance size (CPU, memory, and network capacity).
key_name	The SSH key name used to access the instance securely.
vpc_security_group_ids	Security group(s) controlling network traffic rules for the instance.
availability_zone	(Optional) Specifies the exact availability zone to deploy into.
tags	Metadata for easier resource identification and organization in AWS.
✅ Best Practices

Use Variables: Replace hardcoded values (e.g., instance type, key name) with Terraform variables for flexibility.

Add Outputs: Export instance details like public IP or DNS name for use elsewhere.

Implement Provisioners (Optional): To run initialization scripts (e.g., install web server packages).

Use terraform.tfvars or AWS SSM: For sensitive or environment-specific configuration.

Lock AMI Versions: If stability is important, avoid using dynamic AMI filters that may change unexpectedly.
