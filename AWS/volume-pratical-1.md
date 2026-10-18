### **How to Increase the Size of an Existing EBS Volume in AWS**

#### **Step 1: Modify the Volume Size in the AWS Console**

1. Open the **Amazon EC2 console** at [AWS EC2 Console](https://console.aws.amazon.com/ec2/).
2. In the navigation pane, choose **Elastic Block Store** > **Volumes**.
3. Select the volume you want to expand.
4. Choose **Actions** > **Modify volume**.
5. In the **Size** field, enter the new desired size (in GiB) and choose **Modify**.
6. Confirm the prompt by selecting **Modify**.

#### **Step 2: Connect to Your EC2 Instance via SSH**

```bash
ssh -i "your-key.pem" ec2-user@your-instance-public-ip

```

#### **Step 3: Check Disk and Partition Status**

Run the following command to check the block storage devices and verify that the operating system sees the new allocated size:

```bash
lsblk

```

*Note: You will likely see that the overall disk size (e.g., `nvme0n1` or `xvda`) has increased, but the partition size (e.g., `nvme0n1p1` or `xvda1`) and file system still show the old size.*

#### **Step 4: Grow the Partition**

Depending on your partition table type (MBR or GPT), use the appropriate tool to resize the partition:

* **For most modern Linux systems (using `growpart`):**
First, install cloud-utils-growpart if it's not already installed (e.g., `sudo yum install cloud-utils-growpart` or `sudo apt-get install cloud-guest-utils`), then run:
```bash
sudo growpart /dev/nvme0n1 1

```


*(Replace `/dev/nvme0n1` with your root device name and `1` with the partition number).*

#### **Step 5: Resize the File System**

Extend the file system to fill the newly expanded partition. Check your file system type first (usually `ext4` or `xfs`).

* **For `ext4` file systems:**
```bash
sudo resize2fs /dev/nvme0n1p1

```


* **For `xfs` file systems:**
```bash
sudo xfs_growd -d /

```


*(For XFS, pass the mount point path, typically `/`)*

#### **Step 6: Verify the New Size**

Confirm that the storage expansion was successful across the file system:

```bash
df -h

```
