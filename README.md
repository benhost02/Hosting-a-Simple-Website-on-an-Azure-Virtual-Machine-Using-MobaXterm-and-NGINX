# Hosting a Simple Website on an Azure Virtual Machine Using MobaXterm and NGINX

<h2>Description</h2>
This guide explains how to create a Virtual Machine (VM) in Microsoft Azure using the Azure Portal. A virtual machine allows you to run an operating system and applications in the cloud without managing physical hardware.
<br />

# Creating a Virtual Machine in Microsoft Azure (Step-by-Step)

## Project Overview

This guide explains how to create a **Virtual Machine (VM)** in **Microsoft Azure** using the Azure Portal. A virtual machine allows you to run an operating system and applications in the cloud without managing physical hardware.

---

## Prerequisites

- An active **Microsoft Azure account**
- Basic understanding of cloud concepts
- Internet access

---

## Step 1: Sign in to Azure Portal

1. Open a web browser
2. Go to:
    
    👉 [https://portal.azure.com](https://portal.azure.com/)
    
3. Sign in with your Microsoft account

![vm.png](attachment:64539da0-7ceb-486a-bff6-29853750c125:vm.png)

---

## Step 2: Create a Virtual Machine Resource

1. In the Azure Portal dashboard, click **Create a resource**
2. Select **Virtual Machine**
3. Click **Create**

![vm2.png](attachment:29792ad8-0351-4e05-821d-6fbaf43ff459:vm2.png)

---

## Step 3: Configure Basic Settings

Under the **Basics** tab, fill in the following:

### Subscription & Resource Group

- **Subscription:** Select your Azure subscription
- **Resource Group:**
    - Click **Create new** or select an existing resource group
    - Example: `website-rg`
    
    ![vm3.png](attachment:df0846a9-c10d-40c6-b067-472d9fa4396c:vm3.png)
    

### Virtual Machine Details

- **Virtual Machine Name:** `website-vm`
- **Region:** Choose the nearest or preferred region
- **Availability options:** No infrastructure redundancy required (for learning)
- **Image:** Ubuntu Server 20.04 LTS (or Windows Server if needed)
- **Size:** Standard B1s (cost-effective for learning)

---

## Step 4: Configure Authentication

Choose how you will access the VM:

### For Linux VM:

- **Authentication type:** SSH public key (recommended)
- **Username:** azureuser
- **SSH public key source:** Generate new key pair or use existing key
    
                                   **Or**
    
- **Authentication type:** Password
- **Username & Password:** Create strong credentials

[]()

### For Windows VM:

- **Authentication type:** Password
- **Username & Password:** Create strong credentials

---

## Step 5: Configure Inbound Ports

Under **Inbound port rules**:

- Allow **SSH (22)** for Linux
- Allow **RDP (3389)** for Windows
- Optionally allow **HTTP (80)** if hosting a website

This enables remote access to the VM.

---

## Step 6: Configure Disks

1. Leave default settings for learning purposes
2. **OS Disk type:** Standard SSD (recommended)
3. Click **Next**

![vmn1.png](attachment:92ef5b68-35bb-465c-8162-5da1daa52f3e:vmn1.png)

---

## Step 7: Configure Networking

Azure automatically creates:

- A **Virtual Network (VNet)**
- A **Subnet**
- A **Network Interface (NIC)**

Ensure:

- **Public IP:** Enabled
- **Network Security Group (NSG):** Basic (Allow selected ports)

Click **Next**

![vmn2.png](attachment:5c1bad28-12aa-44a8-9e4e-3ee595862b4c:vmn2.png)

---

## Step 8: Management Settings (Optional)

You may:

- Enable **auto-shutdown** to save costs
- Leave monitoring and backup as default
- Click tag and type the name and the value(example owner, Personal name)

![vmn4.png](attachment:66903f91-9308-4aee-a7fe-4c963b95b47a:vmn4.png)

Click **Next** until you reach **Review + Create**

![vmn5.png](attachment:ee266888-a8d0-4d6a-a407-970da50342a3:vmn5.png)

---

## Step 9: Review and Create

1. Review all configurations
2. Click **Create**
3. If using SSH, download and save the private key securely

Deployment will take a few minutes.

![vmn7.png](attachment:6ce91bb2-c47f-4cf3-bf47-79e3fd302877:vmn7.png)

---

## Step 10: Verify VM Deployment

1. Once deployment completes, click **Go to resource**
2. Confirm:
    - VM status is **Running**
    - Public IP address is assigned

---

![vmn8.png](attachment:61bde2ba-5953-4dbb-b0b4-6c8b3fcb6995:vmn8.png)

## Step 11: Connect to the Virtual Machine

### Linux VM (SSH):

```bash
ssh azureuser@<Public-IP>

```

### Windows VM (RDP):

- Use **Remote Desktop Connection**
- Enter the VM Public IP and credentials

## Step 12: Get the VM Public IP Address

1. Go to the deployed VM
2. Copy the **Public IP address**
    - Example: `20.xxx.xxx.xxx`

This IP will be used to connect and access the website.

---

## Step 13: Connect to Azure VM Using MobaXterm

1. Open **MobaXterm**
2. Click **Session** → **SSH**
3. Enter:
    - **Remote host:** VM Public IP
    - **Username:** azureuser
4. If using SSH key:
    - Browse and select your `.pem` or `.ppk` key
5. If using SSH Password:
    - login with your username and password
6. Click **OK**

You are now connected to your Azure VM.

![mx.png](attachment:90d319a3-375a-4db3-8fdc-75e52a0e4634:mx.png)

![mx5.png](attachment:55bce84d-7287-42e2-8a5f-97fd830fac22:mx5.png)

![mx6.png](attachment:6c2ff614-f06b-453b-9a10-ee769cd096e9:mx6.png)

---

## Step 14: Update the Server

Run the following commands:

```bash
sudo apt update
sudo apt upgrade -y

```

This ensures the system packages are up to date.

![mx7.png](attachment:a1d2d709-528c-43ec-a3c0-45735ec498e0:mx7.png)

---

## Step 15: Install NGINX

Install NGINX web server:

```bash
sudo apt install nginx -y

```

![mxinstallnginx.png](attachment:45481dd4-2d12-45af-885f-5299bc7664a9:mxinstallnginx.png)

Start and enable NGINX:

```bash
sudo systemctl start nginx
sudo systemctlenable nginx

```

Check status:

```bash
sudo systemctl status nginx

```

![mxn2.png](attachment:15772103-7ea9-4ed2-b071-6e75d9ebf8c4:mxn2.png)

---

## Step 16: Create a Simple Website

---

1. Navigate to the NGINX web directory:

```bash
cd /var/www/html

```

1. Edit the default HTML file:

```bash
sudo nano index.html

```

1. Replace the content with:

```html
<!DOCTYPE html>
<html>
<head>
<title>My Azure NGINX Website</title>
</head>
<body>
<h1>Welcome to My Azure VM</h1>
<p>This website is hosted on Azure using NGINX.</p>
</body>
</html>

```

1. Save and exit (`CTRL + O`, `ENTER`, `CTRL + X`)

---

## Step 17: Test the Website

1. Open a browser
2. Enter the VM Public IP:

```
http://<VM-PUBLIC-IP>

```

You should see your website displayed.

## Step 18: Create a Simple Portfolio Website

- Click on the upload button on the mobaxterm
- Select your zip folder for your website
- Click upload butten

![mxupload.png](attachment:9ae46707-f471-45d3-b60e-b9e880dcd5d8:mxupload.png)

- Unzip the the content by using the code `unzip folderName`
- Move the content of the folder to the html folder

![mxupload2.png](attachment:4710952c-ea5d-4f22-b47a-b0f95b942f44:mxupload2.png)

- Remove  the old file index.nginx-debian.html by typying the code `sudo rm filename`

![mxremovefile.png](attachment:d54a7e93-f470-48e3-824a-0c06aa24a986:mxremovefile.png)

- Copy the public ip address and paste it on your url so that you can see the site.

![mxwebsiteRunning.png](attachment:65e6d612-b9b2-4878-808e-b2bd6f2ec0f9:mxwebsiteRunning.png)

## Step 18: Linking To Domain Name Using DNS

- Login to your domain name Service provider

![DNS.png](attachment:0fea9564-8fc7-4d28-8cdc-55f1073a53f1:DNS.png)

- Open the domain name you want to work on
- Go to the DNS section of your domain name manager

![dns2.png](attachment:4e69403d-20c5-4a4a-92d6-648744412e44:dns2.png)

- Create a “A record” add the public ip address and save

![dns4.png](attachment:1f799e69-6c8e-4d9a-9777-5fd61c14d119:dns4.png)

- Copy the domain name  and paste it on your url so that you can see the site.

![dnswebsite.png](attachment:764b625e-17bb-4902-bf58-647c1cc33af1:dnswebsite.png)

---
