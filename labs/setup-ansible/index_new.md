# Getting Started with Ansible

In this hands-on lab, we'll install Ansible on a control node and configure two managed servers for use with Ansible. We will also create a simple inventory and run an Ansible command to verify our configuration is correct.

## Log into the control node
Log in to the control node as `ec2-user` 


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

Click New Repository
name the reposistory lab-setup
Check the `Add a README file` checkbox
Click the `Create Repository` button

In the new repository click the `code` button to expose the https url for the repository
click the copy button to coppy the https url for the repo

## Open the newly create repository in VS Code
launch VS Code from the Start Menu or Task Bar
Select the Source Control Tab from the toolbar on the left
in the Source Control pain click the ... to expand the menu
Select Remote > Add Remote
Paste the URL to newly created Repo



## Create a Simple Ansible Inventory




```
mkdir /home/ansible/lab-setup && cd /home/ansible/lab-setup
```

Next, we'll create a simple Ansible inventory on the control node in `/home/ansible/lab-setup/inventory` containing `node1` and `node2`.

On the control host:

Enter the working directory
```
cd /home/ansible/lab-setup
```
```
touch inventory 
echo "node1 ansible_host=<IP of node1 from spreadsheet>" >> inventory 
echo "node2 ansible_host=<IP of node2 from spreadsheet>" >> inventory 
```



## Configure `sudo` Access for Ansible

Now, we'll configure sudo access for Ansible on `node1` and `node2` such that Ansible may use sudo for any command with no password prompt.

Log in to each managed node as `ec2-user` and edit the `sudoers` file to contain appropriate access for the `ansible` user:

```
ssh ec2-user@node1 
sudo visudo 
```

Add the following line to the file and save:

```
ansible    ALL=(ALL)       NOPASSWD: ALL 
```

Enter:

```
exit
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
