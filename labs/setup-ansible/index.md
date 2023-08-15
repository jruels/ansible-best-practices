# Getting Started with Ansible

In this hands-on lab, we'll install Ansible on a control node and configure two managed servers for use with Ansible. We will also create a simple inventory and run an Ansible command to verify our configuration is correct.

## Log into the control node
Log in to the control node as `ec2-user` 


## Install Ansible on the Control Node

To install Ansible on the control node:

```
sudo dnf install ansible-core -y
```

## Configure the `ansible` user on the Control and Managed Nodes

Next, we'll add a new `ansible` user to each node. This user will be used for running `ansible` tasks. 

On the Controller node run: 
```
sudo useradd ansible
```

Log in to each managed node as the `ec2-user` user and run:

```
sudo useradd ansible
```




Configure the `ansible` user on the control node for ssh shared key access to the managed nodes.

**Note:** Do not use a passphrase for the key pair.

Create a key pair for the `ansible` user on the control host, accepting the defaults when prompted:

```
sudo su - ansible
ssh-keygen 
```



Copy the public key to both nodes provided by the instructor:

On the Control Node copy the output of:

```
cat /home/ansible/.ssh/id_rsa.pub
```



Log in to each of the managed nodes, become the `ansible` user, and add the key to the `authorized_keys` file.


Become the `ansible` user:

```
sudo su - ansible 
```

Use `ssh-keygen`, accepting defaults, to create the `.ssh` directory

```
ssh-keygen
```

Now create the `authorized_keys` file and paste the copied output from above into it.

```
echo "<copied output from above>" > /home/ansible/.ssh/authorized_keys
```

Set the correct permissions

```
chmod 600 /home/ansible/.ssh/authorized_keys
```

Confirm you can ssh as the `ansible` user from the control node to the managed nodes

```
ssh <IP of each node from the spreadsheet>
```



## Create a Simple Ansible Inventory

Create and enter a working directory

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
ssh ec2-user@<IP of node1 from spreadsheet> 
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
