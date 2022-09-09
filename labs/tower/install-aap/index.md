# Ansible Automation Platform

## Install Ansible Automation Platform

### Prerequisites

#### Red Hat Developer account

Go to the [Red Hat Developer portal](https://developers.redhat.com/about), click "Join now," and fill out the form. 

Provide the following: 

* Username 
* Email address 
* Job role 
* Password 

#### Red Hat registry service account token

Navigate to the [Registry Service Account Management Application](https://access.redhat.com/terms-based-registry/), and log in if necessary.

1. From the **Registry Service Accounts** page, click the **New Service Account** button.

2. Provide a name for the Service Account. It will be prepended with a fixed, random string.

   - Enter a description.
   - Click **create**.

3. Navigate back to your Service Accounts.

4. Click the Service Account you created.

   - Note the username, including the prepended string (i.e. `XXXXXXX|username`). This is the username that should be used to log in to registry.redhat.io.
   - Note the password. This is the password that should be used to authenticate to [registry.redhat.io.](https://registry.redhat.io) in the `inventory` file.

   



### Connect to Lab environment

At the top of the lab page, right-click "View on GitHub" and open in a new tab. 

In the new GitHub tab, click the green "Code" button, and in the drop-down, click "Download zip"

Extract the zip file. The extracted directory contains a "keys" directory.

#### macOS

On macOS and Linux use the `tower` file

#### Set permission on SSH key

```
chmod 600 /path/to/tower
```

#### SSH to lab servers

The username for SSH is `ansible`

```
ssh -i /path/to/tower ansible@<LAB IP> 
```



#### Windows

Open Putty and configure a new session.

![img](https://jruels.github.io/int-docker-kube/labs/access_docker/index/C4EC1E64-175D-4C84-8C49-D938337FA35A.png)



Expand "Connection_SSH_Auth" and then specify the 'tower.ppk' file.



![img](https://jruels.github.io/int-docker-kube/labs/access_docker/index/6FFB137C-1AD8-48A1-97E6-F5F6DA4BC55B.png)



Now save your session

![img](https://jruels.github.io/int-docker-kube/labs/access_docker/index/FD3BA694-FD69-4C86-8EAF-4D5FC813EABA.png)



When connecting provide the username `ansible`

### Install Automation Platform 

Install `subscription-manager`

```bash
sudo yum install -y subscription-manager
```



Register VM with Red Hat package repos

```bash
sudo subscription-manager register --auto-attach
```



When prompted, provide your Red Hat Developer username and password.

Click [Here](https://access.redhat.com/downloads/content/480) to download the Ansible Automation Platform installer. 

* Log in to the portal 

* Click the "Download Now" link

  ![image-20220221230429201](images/image-20220221230429201.png)

Copy the installer to the lab VM using WinSCP:



Create a new connection to the lab server:

- Change File protocol to SCP
- Enter the Hostname of the Lab VM
- Enter the username provided by the instructor

![img](https://winscp-static-746341.c.cdn77.org/data/media/screenshots/login.png?v=6990)



Configure SSH Key Authentication:

* Browse the key provided by the instructor.

![img](https://winscp-static-746341.c.cdn77.org/data/media/screenshots/login_authentication.png?v=6990)



After connecting, copy the download setup file to the Lab VM in `/home/ansible`

<img src="https://winscp-static-746341.c.cdn77.org/data/media/screenshots/commander.png?v=6990" alt="img" style="zoom:85%;" />



Connect to the server using SSH



Extract the installer 

```bash
tar -xvf ansible-automation-platform-setup-2.1.1-1.tar
```



Enter directory 

```bash
cd ansible-automation-platform-setup-2.1.1-1
```



Set the following in the `inventory` file 

* Admin password 
* pg_password
* registry_username 
* registry_password 

Sample inventory 

```
[automationcontroller]
127.0.0.1 ansible_connection=local

[database]

[all:vars]
admin_password='Password1234'

pg_host=''
pg_port=''

pg_database='awx'
pg_username='awx'
pg_password='Password1234'

registry_url='registry.redhat.io'
registry_username='15765574|ansible-tower'
registry_password='eyJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJlZTBmNDllNzE1ZWQ0MmQ3YjZkNzk2MWRiZDA1ZDgwNCJ9.APx3LOHNzp4O2pNewBL5X2CR84vJqL24wPBuEtUfQUU6ckP4CJcYIl2lyr9NQCcapG1rtoTiAp4NHstmroi-m_nCOcr1he8vMsUSNGMKqT-tBr-oe84bENmYM_DNvbQReh1pW4c6D2VQFXC9iN60vNl4cylBSy4BWFeg3Dgj62RQSgDAr8RX4twjh0BDbIE75IcVLK2cyvHTqmSst9kk0q9_iAAvGokxRkEHwurkPg6VCZ0MFG3O6GRtVCVS055mGR6P48Wzhwvm698GKHferVyt-0AqWox83hvQWB-pvRmOgkZyFnsdlHpHNIzCN1_g7LQmkk-Lqw0NYanqU-ag8yHnv_RO4HpOI-PAWponrFk7lU3HRBOAjeyHiW60FwgTtDOftTi7Gz-Uer8mcZfLoyMfByYoK8BUc7lNhCPvG32qkEMkbdB7TFSRmIVqgh3VQ3m8OYCu7IrD3qpLA3faydQIqa8h_XMs9umrSvpb7kctDVrTqwQvZp3rZqZbJE5-qL_xwdgdTCZcVE5JBa8Rhr962K5sLVJLfWRqkAoK3agmzXFYbl16VNtoAfeMblyOK_jQc-WMycxr7C0aQAHQ18Y7zRBVJyRplwyuc1k-zjcZA9GBvhW35fF-NyNIMF2qga-Rz2z_0zOw8zkP1Ioug--sRz8Tgz47UMphlkm'
```



Run installation script

```bash
sudo ./setup.sh -e required_ram=2048
```





**NOTE**: This will take some time to complete.



After the script above completes you can access the Dashboard at the following URL (replacing `Server IP` with your lab VM IP)  

https://[Server IP]/#/login



Log into the dashboard with the username `admin` and the password you specified in the `inventory` file



> NOTE: You can reset the admin password by running the following on the Lab VM:
>
> ```bash
> sudo awx-manage changepassword admin
> ```



You will see a screen asking to register Automation Platform. Log in with your developer credentials.

<img src="images/image-20220222022946979.png" alt="image-20220222022946979" style="zoom: 33%;" />



Select your subscription, and click "Next"

<img src="images/image-20220222024205420.png" alt="image-20220222024205420" style="zoom:50%;" />

Accept defaults, and click "Next"

<img src="images/image-20220222024307322.png" alt="image-20220222024307322" style="zoom:45%;" />

Accept the license agreement and click "Submit"



You should now see the dashboard 

![image-20220222024405897](images/image-20220222024405897.png)

### Congratulations
