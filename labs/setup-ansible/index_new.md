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
You may need to reconnect to the Ubuntu server with Putty
From the PuTTY menu select Restart Session

Install Win RM
```
pip3 install "pywinrm>=0.3.0"
```

Install boto3 and botocore

```
pip3 install boto3 botocore
```

## Create a new Reposistory in your personal GitHub Account

1. Click New Repository
1. Name the reposistory lab-setup
1. Check the `Add a README file` checkbox
1. Click the `Create Repository` button
1. In the new repository click the `code` button to expose the https url for the repository
1. Click the copy button to coppy the https url for the repo

## Open the newly create repository in VS Code

1. Launch VS Code from the Start Menu or Task Bar
1. Select the Source Control Tab from the toolbar on the left
1. In the Source Control pain click `Clone Repository`
1. Paste the URL to newly created Repo

## Create a Simple Ansible Inventory

Next, we'll create a simple Ansible inventory on the control node in `C:\GitRepos\ansible-working\lab-setup\inventory.yml` containing `webserver1` and `webserver2`.

In VS Code 

Enter the working directory
```
cd /home/ansible/lab-setup
```
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



## Configure `sudo` Access for Ansible

Now, we'll configure WinRM for each windows node by creating a key using it to create a listener then opening the ports on the firewall

Remote desktop into the Windows node and run the powershell script to enable WinRM

1. In remote desktop enter the ip address for Windows Target 1
2. Click `Show Options`
3. Enter `Administrator` as the user name
4. Click `Connect`
5. Enter `JustM300` as the password
6. Click `Yes`
7. Open Chrome and use the link below to download the powershell script 

```

Repeat these steps for `node2`

## Verify Each Managed Node Is Accessible

Finally, we'll verify each managed node is able to be accessed by Ansible from the control node using the `ping` module.

Redirect the output of a successful command to `/home/ansible/lab-setup/output`.

To verify each node, run the following as the `ansible` user from the control host:

Enter the working directory:
```
cd /home/ansible/lab-setup
```

```
ansible -i inventory node1 -m ping 
ansible -i inventory node2 -m ping 
```

To redirect output of a successful command to `/home/ansible/lab-setup/output`:

```
ansible -i inventory node1 -m ping > output 
```

## Conclusion

Congratulations on completing this lab!
