# Getting Started with Ansible

In this hands-on lab, we'll install Ansible on a control node and configure two managed servers for use with Ansible. We will also create a simple inventory and run an Ansible command to verify our configuration is correct.

## Log into the control node

Perform the following steps in the RPD session to Windows Target 1

1. Launch PuTTY
1. Load Default settings
1. Paste ip address (provided by instructor)
1. Click Open
1. Click Yes to accept the cert

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

> You may need to reconnect to the Ubuntu server with Putty. From the PuTTY menu select Restart Session

Install Win RM

```
pip3 install "pywinrm>=0.3.0"
```

Install boto3 and botocore

```
pip3 install boto3 botocore
```

In the VS Code Explorer pane:

1. Right Click in the explorer pane
1. Select `New File`
1. Name the new file 'inventory_simple.yml'
1. Paste the code below into the file

    ```
    ---
    webservers:
      hosts:
        webserver1:
          ansible_host: <ip address provided>
          ansible_user: Administrator
          ansible_password: JustM300
          ansible_connection: winrm
          ansible_winrm_transport: ntlm
          ansible_winrm_server_cert_validation: ignore
        webserver2:
          ansible_host: <ip address provided>
          ansible_user: Administrator
          ansible_password: JustM300
          ansible_connection: winrm
          ansible_winrm_transport: ntlm
          ansible_winrm_server_cert_validation: ignore
    ```
          
> In the real world we would not want to store the windows credentials in plain text in our inventory file. We will deal with this issue in the vault lab.    
    
### Commit and Push Changes to GitHub

1. In the sidebar, click on the "Source Control" icon (it looks like a branch).
2. In the "Source Control" pane, review the changes you made to the file.
3. Enter a commit message that describes the changes you made.
4. Click the checkmark icon to commit the changes.
5. Click on the "..." menu in the "Source Control" pane, and select "Push" to push the changes to GitHub.

## Update the Ansible Control Host

1. Return to the connection to your Ansible control host in PuTTY on Windows Target 1.
2. Navigate to the directory where you cloned repository.
3. Run `git pull` to update the repository on the control host.

## Verify Each Managed Node Is Accessible

Here we will attempt to verify each managed node is able to be accessed by Ansible from the control node using the `win_ping` module.

To verify each node, run the following as the `ansible` user from the `Ansible Control` host:

First we will clone the `ansible-working` repository you created earlier. Return to GitHub and copy the https url to your `ansible-working` repository. Clone the `ansible-working` repository to retrieve our `inventory_simple.yml` file.

```
cd /home/ansible
git clone https://github.com/<Your Account>/ansible-working.git
```

Enter the working directory and ping the webservers:

```
cd ansible-working
ansible -i inventory_simple.yml webserver1 -m win_ping 
ansible -i inventory_simple.yml webserver2 -m win_ping 
```

> This will fail because we have not yet enabled WinRM or opened its ports on the firewall.
  
## Enable WinRM on Windows Targets

Now, we'll configure WinRM for each windows node by creating a key using it to create a listener then opening the ports on the firewall.

Perform the following steps in the RDP session for Windows Target 1 then repeat by opening another Remote Desktop Session to Windows Target 2.

In the VS Code Explorer pane:

1. Right Click in the explorer pane
1. Select `New File`
1. Name
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
  Restart-Service winrm
  #Repeat these steps for `Windows Target 2
  ```
    
5. Execute the Script 
1. Save, Commit and Sync the PowerShell Script with GitHub

Repeat these steps on Windows Target 2 by opening a new Remote Desktop Session to Windows Target 2

## Verify Each Managed Node Is Accessible (Again)

Lets use the `win_ping` module again to enure that we can access the Windows Targets on their newly enabled listeners.
To verify each node, use the win_ping module again:

  ```
  ansible -i inventory_simple.yml webserver1 -m win_ping 
  ansible -i inventory_simple webserver2 -m win_ping 
  ```

  > This will succeed now because WinRM is enabled and its ports are opened on the firewall

  To redirect output of a successful command to `/home/ansible/ansible-working/output`:

  ```
  ansible -i inventory webserver1 -m win_ping > output 
  ```

## Conclusion

  Congratulations on completing this lab!
