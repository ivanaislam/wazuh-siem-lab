# Wazuh SIEM Deployment on Ubuntu Server 22.04

This is my lab documentation for installing Wazuh SIEM on Ubuntu Server 22.04 using VirtualBox and connecting through PuTTY.

---

## What I Used

- Oracle VirtualBox
- Ubuntu Server 22.04.5 LTS (Jammy Jellyfish)
- PuTTY (for SSH connection)
- Wazuh v4.14.5

---

## Step 1 - Install Oracle VirtualBox

First I downloaded and installed Oracle VirtualBox from the official website.

![virtualbox install](image1.png)

Then I installed the VirtualBox Extension Pack.

![extension pack](image2.png)

---

## Step 2 - Download Ubuntu Server ISO

I downloaded Ubuntu Server 22.04.5 LTS server install image.

![ubuntu download](image3.png)

I installed it in D drive because Wazuh takes a lot of disk space.

---

## Step 3 - Create Virtual Machine in VirtualBox

Open VirtualBox and select New to create a new VM.

![new vm](image4.png)

![select iso](image5.png)

Select the Ubuntu ISO image and set the folder location.

![uncheck box](image6.png)

Uncheck the auto install box and click Finish.

![settings](image7.png)

---

## Step 4 - Configure VM Settings

Select the VM and go to Settings.

![memory cpu](image8.png)

![network](image9.png)

Allocate base memory 4096 MB and CPU 2.

![port forward](image10.png)

![fill boxes](image11.png)

---

## Step 5 - Configure Network (Port Forwarding)

Go to Network settings and configure port forwarding so we can connect via PuTTY.

![start machine](image12.png)

![language](image13.png)

Click the green icon twice to add port forwarding rules.

![profile setup](image14.png)

Fill in the port forwarding boxes:
- SSH: Host port 2222 -> Guest port 22
- HTTPS: Host port 443 -> Guest port 443

![installing](image15.png)

![reboot](image16.png)

---

## Step 6 - Install Ubuntu Server

Start the virtual machine.

![press enter](image17.png)

![putty](image18.png)

Select language.

![accept](image19.png)

No update needed.

![no update](image20.png)

Keyboard configuration.

![keyboard config](image21.png)

Type of installation.

![installation type](image22.png)

Network configuration.

![network config](image23.png)

Proxy configuration.

![proxy config](image24.png)

Mirror configuration.

![mirror config](image25.png)

Storage configuration.

![storage config](image26.png)

File system summary.

![filesystem summary](image27.png)

Confirm destructive action.

![confirm action](image28.png)

Set up your profile - username and password.

![profile](image29.png)

![profile 2](image30.png)

Upgrade to Ubuntu Pro (skip).

![ubuntu pro](image31.png)

SSH configuration.

![ssh config](image32.png)

Featured server snaps.

![server snaps](image33.png)

It takes 5 to 10 minutes to complete the installation.

Installing system.

![installing system](image34.png)

Installation complete.

![install complete](image35.png)

Click Reboot Now when installation is done.

![reboot now](image37.png)

Press Enter after reboot and wait 2 minutes.

---

## Step 7 - Connect via PuTTY

Minimize Ubuntu and open PuTTY.

Enter the connection details:
- Host Name: `127.0.0.1`
- Port: `2222`
- Connection type: SSH

![putty config](image38.png)

Click Accept on the security alert.

![security alert](image39.png)

Login with Ubuntu username and password.

![ubuntu login](image40.png)

---

## Step 8 - Install Wazuh SIEM

Download the Wazuh installation script:

```bash
curl -sO https://packages.wazuh.com/4.10/wazuh-install.sh
```

Set the timeout so installation does not fail:

```bash
export WAZUH_READY_TIMEOUT=3000
```

Run the installation:

```bash
sudo -E bash wazuh-install.sh -a -o
```

![wazuh install start](image41.png)

Give password and press Enter. Wait 15 to 20 minutes for installation to complete.

Wazuh has 3 main components that get installed:
1. Wazuh Indexer
2. Wazuh Manager (Server)
3. Wazuh Dashboard

---

## Step 9 - Verify Services are Running

After installation, check that all services are running:

```bash
sudo systemctl status wazuh-indexer wazuh-manager wazuh-dashboard --no-pager
```

---

## Step 10 - Access Wazuh Dashboard

Open browser and go to:

```
https://127.0.0.1
```

![browser open](image42.png)

Click Advanced and then click Proceed (because it uses a self-signed certificate).

![browser warning](image43.png)

Login with:
- Username: `admin`
- Password: *(from wazuh-passwords.txt file)*

```bash
sudo cat ~/wazuh-install-files/wazuh-passwords.txt
```

![login page](image44.png)

---

## Final Result

After successful login the Wazuh dashboard is fully working.

![wazuh overview](image45.png)

The Wazuh SIEM is now fully deployed and running. It includes:
- Endpoint Security (malware detection, file integrity monitoring)
- Threat Intelligence (MITRE ATT&CK, vulnerability detection)
- Security Operations (GDPR, HIPAA, PCI DSS compliance)
- Cloud Security monitoring

---

## Commands Used

```bash
# Download installation script
curl -sO https://packages.wazuh.com/4.10/wazuh-install.sh

# Set timeout
export WAZUH_READY_TIMEOUT=3000

# Run installation
sudo -E bash wazuh-install.sh -a -o

# Check services
sudo systemctl status wazuh-indexer wazuh-manager wazuh-dashboard

# Get admin password
sudo cat ~/wazuh-install-files/wazuh-passwords.txt
```

---

*Lab completed by Ivana Islam*
