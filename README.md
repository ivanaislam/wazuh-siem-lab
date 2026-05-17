# Wazuh SIEM Deployment on Ubuntu Server 22.04

This is my lab documentation for installing Wazuh SIEM on Ubuntu Server 22.04 using VirtualBox and connecting through PuTTY.

---

## What i used

- Oracle VirtualBox
- Ubuntu Server 22.04.5 LTS (Jammy Jellyfish)
- PuTTY (for SSH connection)
- Wazuh v4.14.5

---

## Step 1 - Install Oracle VirtualBox

First i downloaded and installed Oracle VirtualBox from the official website.

![virtualbox install](image1.png)

Then i installed the VirtualBox Extension Pack.

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

Select the ubuntu iso image and set the folder location.

![select iso](image5.png)

Uncheck the auto install box and click finish.

![uncheck box](image6.png)

---

## Step 4 - Configure VM Settings

Select the VM and go to Settings.

![settings](image7.png)

Allocate base memory 4096 MB and CPU 2.

![memory cpu](image8.png)

---

## Step 5 - Configure Network (Port Forwarding)

Go to Network settings and configure port forwarding so we can connect via PuTTY.

![network](image9.png)

Click the green icon twice to add port forwarding rules.

![port forward](image10.png)

Fill in the port forwarding boxes:
- SSH: Host port 2222 -> Guest port 22
- HTTPS: Host port 443 -> Guest port 443

![fill boxes](image11.png)

---

## Step 6 - Install Ubuntu Server

Start the virtual machine.

![start machine](image12.png)

Select language.

![language](image13.png)

Set up your profile - username and password.

![profile setup](image14.png)

It takes 5 to 10 minutes to complete the installation.

![installing](image15.png)

Click Reboot Now when installation is done.

![reboot](image16.png)

Press Enter after reboot and wait 2 minutes.

![press enter](image17.png)

---

## Step 7 - Connect via PuTTY

Minimize ubuntu and open PuTTY.

![putty](image18.png)

Enter the connection details:
- Host Name: 127.0.0.1
- Port: 2222
- Connection type: SSH

Click Accept on the security alert.

![accept](image19.png)

Login with ubuntu username and password.

![login](image20.png)

---

## Step 8 - Install Wazuh SIEM

Download the Wazuh installation script:

```bash
curl -sO https://packages.wazuh.com/4.10/wazuh-install.sh
```

Set the timeout so installation doesnt fail:

```bash
export WAZUH_READY_TIMEOUT=3000
```

Run the installation:

```bash
sudo -E bash wazuh-install.sh -a -o
```

![wazuh install start](image21.png)

Give password and press enter. Wait 15 to 20 minutes for installation to complete.

![installing wazuh](image22.png)

![wazuh progress](image23.png)

Wazuh has 3 main components that get installed:
1. Wazuh Indexer
2. Wazuh Manager (Server)
3. Wazuh Dashboard

![components installing](image24.png)

---

## Step 9 - Verify Services are Running

After installation check that all services are running:

```bash
sudo systemctl status wazuh-indexer wazuh-manager wazuh-dashboard --no-pager
```

![services status](image25.png)

---

## Step 10 - Access Wazuh Dashboard

Open browser and go to:
```
https://127.0.0.1
```

Click Advanced and then click Proceed (because it uses a self-signed certificate).

![browser warning](image26.png)

Login with:
- Username: admin
- Password: (from wazuh-passwords.txt file)

```bash
sudo cat ~/wazuh-install-files/wazuh-passwords.txt
```

![login page](image27.png)

---

## Final Result

After successful login the Wazuh dashboard is fully working.

![wazuh overview](image28.png)

![endpoint security](image29.png)

The Wazuh SIEM is now fully deployed and running. It includes:

- Endpoint Security (malware detection, file integrity monitoring)
- Threat Intelligence (MITRE ATT&CK, vulnerability detection)
- Security Operations (GDPR, HIPAA, PCI DSS compliance)
- Cloud Security monitoring

![full dashboard](image30.png)

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
