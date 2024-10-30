# CloudFormation Template: EC2, VPC, EFS, and S3 Bucket Setup

This CloudFormation template provisions an AWS infrastructure with a custom Virtual Private Cloud (VPC), EC2 instance, Elastic File System (EFS), and an S3 bucket with access permissions for the EC2 instance. The EC2 instance is configured to mount the EFS filesystem and access objects in the S3 bucket.

## Resources Created

The CloudFormation template creates the following resources:

- **VPC**: A custom VPC with both public and private subnets.
- **Internet Gateway**: Enables internet access for public resources.
- **Public and Private Subnets**: Provides separate networks for publicly and privately accessible resources.
- **Security Groups**:
    - **EFS Security Group**: Restricts access to the EFS filesystem.
    - **EC2 Security Group**: Allows SSH access and NFS access to EFS.
- **Elastic File System (EFS)**: A shared file system mounted to the EC2 instance.
- **IAM Role and Instance Profile**: Grants the EC2 instance permissions to access the S3 bucket.
- **EC2 Instance**: A single Amazon Linux 2 instance, configured with user data to mount the EFS filesystem on boot.

## Parameters

- **KeyName**: The name of an existing EC2 KeyPair to enable SSH access to the instance.
- **ImageId**: The Amazon Machine Image (AMI) ID for the EC2 instance (default: Amazon Linux 2).
- **InstanceType**: The instance type of the EC2 instance (default: `t2.micro`).

## Usage

1. **Prepare the Parameters**:
    - Make sure you have an existing KeyPair for SSH access.
    - Customize the instance type and AMI if necessary.

2. **Deploy the Template**:
    - Open the [AWS CloudFormation Console](https://console.aws.amazon.com/cloudformation).
    - Choose **Create Stack** and select **Template is ready**.
    - Upload this CloudFormation template or specify the template’s URL if hosted remotely.

3. **Access the EC2 Instance**:
    - After the stack is created, find the EC2 instance’s public IP in the CloudFormation Outputs section.
    - Connect to the instance using SSH with the KeyPair specified in `KeyName`:

      ```bash
      ssh -i path/to/your-key.pem ec2-user@<EC2_Instance_Public_IP>
      ```

4. **Verify EFS Mount**:
    - On the EC2 instance, verify that the EFS is mounted:

      ```bash
      df -h | grep efs
      ```

5. **S3 Bucket Access**:
    - The IAM role attached to the instance allows access to the S3 bucket. Use the AWS CLI or SDK to interact with the bucket. Example:

      ```bash
      aws s3 ls s3
      ```

## Notes

- **Security**: Update the security group settings as needed. For production, consider restricting SSH access to trusted IP ranges.
- **IAM Policies**: The S3 access policy grants the EC2 instance permissions to list, get, and put objects in the specified bucket.
- **Availability Zones**: Ensure the subnets are in availability zones available in your selected region.

## Cleanup

To avoid charges, delete the CloudFormation stack once you are done:

1. Go to the CloudFormation Console.
2. Select your stack and click **Delete**.

---

This template sets up a basic yet customizable AWS infrastructure. Modify and extend it as needed to fit your application's requirements.
