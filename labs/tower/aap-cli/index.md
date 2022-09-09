# AAP - AWX CLI

## Objective

This is an advanced lab so we don’t really want you to use the web UI for everything. AAP's web UI is well done and helps with a lot of tasks, but same as in system administration it’s often handy to be able to use the command line or scripts for certain tasks.

We’ve incorporated different ways to work with AAP in this lab guide and hope you’ll find it helpful. The first step we do is install the **AWX CLI** utility.

We must install the package as root.

> **TIP:** **AWX CLI** is the official command-line client for AWX and Red Hat Ansible Tower. It uses naming and structure consistent with the AWX HTTP API, provides consistent output formats with optional machine-parsable formats.

```bash
sudo su - 
yum-config-manager --add-repo https://releases.ansible.com/ansible-tower/cli/ansible-tower-cli-el8.repo
yum install ansible-tower-cli -y
exit
```



> **TIP:** Remember to exit as the root user



```bash
export TOWER_HOST=https://<TOWER VM IP>
export TOWER_USERNAME=admin
export TOWER_PASSWORD=<YOUR PASSWORD, prob Password1234>
export TOWER_VERIFY_SSL=false
```



Use `awx` to login and print out the access token and save it to a file at the same time:

```bash
awx login -f human | tee token
```



> **TIP:** We are saving the **export TOWER_OAUTH_TOKEN=<YOUR_TOKEN>** command line output to the file **token** using **tee** here to be able to set the environment variable more easily.



Finally set the environment variable with the token using the line the command printed out:



```bash
source token
```



Now that the access token is available in your shell, test **awx** is working. First run it without arguments to get a list of resources you can manage with it:

``` bash
awx --help
```



And then test something, e.g. (leave out **-f human** if you’re a JSON fan)

```bash
awx -f human user list
```



The `awx` command has built-in help documentation. You can access it adding `--help`to any command, or not passing any arguments, for example: 

```bash
awx
```



Now that we know there is an inventory option we can review the options. 

```bash
awx inventory
```



To create a new inventory we can use: 

```bash
awx inventory create
```



## Challenge Lab: awx

To practice your **awx** skills, here is a challenge:

- Try to change the **idle time out** of the Tower web UI, it’s `1800` seconds by default. Set it to, say, `7200`. Using **awx**, of course.
- Start by looking for a resource type **awx** provides using **–help** that sounds like it has something to do with changing settings.
- Look at the available **awx** commands for this resource type.
- Use the commands to have a look at the parameters settings and change it.



## Congrats! 



