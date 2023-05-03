# Getting Started with Ansible

In this hands-on lab, we'll install Ansible on a control node and configure two managed servers for use with Ansible. We will also create a simple inventory and run an Ansible command to verify our configuration is correct.

## Log into the control node
Log in to the control node as `ubuntu` 


## Install Ansible on the Control Node

Get updates:

  ```
  sudo apt-get update
  ```

Install Python

  ```
  sudo apt-get install -y python3-pip
  ```

Install Ansible using pip3

  ```
  pip3 install ansible
  ```

Reboot Ubuntu to update the path and complete Ansible setup

  ```
  sudo reboot
  ```

> You may need to reconnect to the Ubuntu server with Putty
> From the PuTTY menu select Restart Session

Install Win RM

  ```
  pip3 install "pywinrm>=0.3.0"
  ```

Install boto3 and botocore

  ```
  pip3 install boto3 botocore
  ```
## Connect to Windows Target 1

We will do most of our work on Windows Target 1

Use Remote Desktop to remote into Windows Target 1 (ip address provided)

  1. In remote desktop enter the ip address for Windows Target 1
  2. Click `Show Options`
  3. Enter `Administrator` as the user name
  4. Click `Connect`
  5. Enter `JustM300` as the password
  6. Click `Yes`

Repeat these steps to Open a RDP Session to Target 2 while keeping this session open

## Clone the ClassFiles Repo

Continue to do the following steps inside of Window Target 1 RDP Session

  1. Launch VS Code from the Start Menu or Task Bar
  1. Select the Source Control Tab from the toolbar on the left
  1. In the Source Control pain click `Clone Repository`
  1. Paste the URL https://github.com/ProDataMan/ansible-best-practices-windows.git 
  1. Enter your GitHub credentials
  1. Select the `lab-setup` repository you created earlier
  1. In the choose a folder dialog navigate to `c:\GitRepos`
  1. Click the `select as Repository Destination` button
  1. In the Visual Studio Code dialog click the `Open` button to open the repository in VS Code

## Create a new Reposistory in your personal GitHub Account

Continue to do the following steps inside of Window Target 1 RDP Session

  1. Using Chrome, Log in or Create a new account at `www.GitHub.com`
  1. Click New Repository
  1. Name the reposistory `lab-setup`
  1. Check the `Add a README file` checkbox
  1. Click the `Create Repository` button
  1. In the new repository click the `code` button to expose the `https url` for the repository
  1. Click the copy button to copy the `https url` for the repo to use in the next step

## Open the newly create repository in VS Code

  1. Launch VS Code from the Start Menu or Task Bar
  1. Select the Source Control Tab from the toolbar on the left
  1. In the Source Control pain click `Clone Repository`
  1. Paste the URL to newly created Repo
  1. Enter your GitHub credentials
  1. Select the `lab-setup` repository you created earlier
  1. In the choose a folder dialog navigate to `c:\GitRepos`
  1. Click the `select as Repository Destination` button
  1. In the Visual Studio Code dialog click the `Open` button to open the repository in VS Code
  1. In the Toolbar click the  Explorer button

## WinRM on Windows Targets

Now, we'll configure WinRM for each windows node by creating a key using it to create a listener then opening the ports on the firewall

Continue performing the following steps in the RDP session for Windows Target 1 then Repeat by Opening another Remote Desktop Session to Windows Target 2

In the VS Code Explorer pane

  1. Right Click in the explporer pane
  1. Select `New File`
  1. Name the new file ConfigureWindowsTargets.ps1
  1. Paste the code below into the file
    ```
     #Allow unencrypted connections
      winrm set winrm/config/service '@{AllowUnencrypted="true"}'
     # Create self-signed certificate
      $cert = New-SelfSignedCertificate -DnsName "localhost" -CertStoreLocation "cert:\LocalMachine\My"
     # Get the certificate thumbprint
      $thumbprint = $cert.Thumbprint
     # Create a new listener using the certificate thumbprint
      New-Item -Path WSMan:\localhost\Listener -Transport HTTPS -Address * -CertificateThumbPrint $thumbprint -Force
     # Create a windows defender firewall inbound rule for ports 5985,5986
      New-NetFirewallRule -DisplayName "Allow Ansible" -Direction Inbound -LocalPort 5985,5986 -Protocol TCP -Action Allow
     # Turn on PowerShell Remoting
      Enable-PSRemoting -Force
     # Restart WinRM
      Restart-Service winrmRepeat these steps for `Windows Target 2`
    ```
   1. Execute the Script 
   1. Save, Commit and Sync the PowerShell Script with GitHub
   
Continue performing the following steps in the RDP session for Windows Target 1 then Repeat by Opening another Remote Desktop Session to Windows Target 2

In the VS Code Explorer pane

  1. Right Click in the explporer pane
  1. Select `New File`
  1. Name the new file 'inventory_simple.yml
  1. Paste the code below into the file
    ```
    ---
    webservers:
      hosts:
        webserver1:
          ansible_host: 52.53.151.241
          ansible_user: Administrator
          ansible_password: JustM300
          ansible_connection: winrm
          ansible_winrm_transport: ntlm
          ansible_winrm_server_cert_validation: ignore
        webserver2:
          ansible_host: 54.183.197.81
          ansible_user: Administrator
          ansible_password: JustM300
          ansible_connection: winrm
          ansible_winrm_transport: ntlm
          ansible_winrm_server_cert_validation: ignore
    ```
   1. Save, Commit and Sync the inventory file with GitHub

## Verify Each Managed Node Is Accessible

Finally, we'll verify each managed node is able to be accessed by Ansible from the control node using the `ping` module.

To verify each node, run the following as the `ansible` user from the control host:

First we will clone the ansible-working repository you created earlier

Return to GitHub and copy the https url to your ansible-working repository

Clone the ansible-working repository to retrive our inventory_simple.yml file
```
cd /home/ansible
git clone https://github.com/<Your Account>/ansible-working.git
```

  Enter the working directory and ping the webservers
```
  cd ansible-working
  ansible -i inventory_simple.yml webserver1 -m win_ping 
  ansible -i inventory_simple webserver2 -m win_ping 
```

To redirect output of a successful command to `/home/ansible/ansible-working/output`:

```
ansible -i inventory webserver1 -m win_ping > output 
```

## Conclusion

Congratulations on completing this lab!
