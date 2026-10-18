### **How to Create and Attach an External EBS Volume to an EC2 Instance**

#### **Step 1: Create a New EBS Volume**

1. Open the **Amazon EC2 console** at [AWS EC2 Console](https://console.aws.amazon.com/ec2/).
2. In the navigation pane, choose **Elastic Block Store** > **Volumes**.
3. Click on **Create volume**.
4. Configure your volume settings:
* **Volume type:** Select your desired type (e.g., General Purpose SSD `gp3`).
* **Size (GiB):** Specify the storage capacity you need.
* **Availability Zone:** **Important:** The volume *must* be created in the exact same Availability Zone (e.g., `us-east-1a`) as the target EC2 instance.


5. Click **Create volume**.

#### **Step 2: Attach the Volume to the EC2 Instance**

1. Once the volume state changes to `available`, select the newly created volume.
2. Click **Actions** > **Attach volume**.
3. In the **Instance** search box, select the target EC2 instance.
4. Note the **Device name** (e.g., `/dev/sdf` or `/dev/xvdf`). AWS will map this inside the operating system.
5. Click **Attach volume**.

#### **Step 3: Connect to Your EC2 Instance via SSH**

```bash
ssh -i "your-key.pem" ec2-user@your-instance-public-ip

```

#### **Step 4: Verify the Attached Disk**

Run the following command to list all available storage disks and check if your new volume is connected:

```bash
lsblk

```

*(Look for your new unformatted disk, such as `xvdf` or `nvme1n1`, which won't have any partitions or mount points listed yet).*

#### **Step 5: Check if the Volume Has a File System**

Check if the disk already contains data or a file system:

```bash
sudo file -s /dev/xvdf

```

* If it returns `/dev/xvdf: data`, the volume is blank and ready to be formatted.
* If it returns a file system type (like `ext4`), skip to Step 7.

#### **Step 6: Format the Volume (If Blank)**

Create an `ext4` (or `xfs`) file system on the volume:

```bash
sudo mkfs -t ext4 /dev/xvdf

```

#### **Step 7: Mount the Volume**

1. Create a directory folder where you want to mount the new storage:
```bash
sudo mkdir /mnt/my-data

```


2. Mount the volume to that directory:
```bash
sudo mount /dev/xvdf /mnt/my-data

```



#### **Step 8: Verify the Mount**

Check that the storage is successfully mounted and view available disk space:

```bash
df -h

```

#### **Step 9: (Optional) Configure Automatic Mounting on Reboot**

To ensure the volume automatically remounts if the EC2 instance restarts:

1. Back up your `fstab` file:
```bash
sudo cp /etc/fstab /etc/fstab.bak

```


2. Get the volume's UUID:
```bash
sudo blkid

```


3. Open the `/etc/fstab` file using a text editor (like `nano`):
```bash
sudo nano /etc/fstab

```


4. Add a new line at the bottom using the UUID (replace with your actual UUID and file system type):
```text
UUID=your-volume-uuid-here  /mnt/my-data  ext4  defaults,nofail  0  2

```


5. Save and close the file, then test your configuration:
```bash
sudo mount -a

```
