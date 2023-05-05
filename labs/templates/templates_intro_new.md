## Lab Title: Using Templates to Create Configuration Files

### Lab Objective:
In this lab, you will learn how to use Ansible's template module to create configuration files based on a template file and variables. You will create a template file and use it to generate a configuration file on a Windows host.

### Prerequisites:
- An AWS account
- A running Windows EC2 instance named "Windows Target 1"
- Git and Visual Studio Code installed on "Windows Target 1"
- "ansible-working" repository cloned and opened in Visual Studio Code on "Windows Target 1"
- "ansible" and "boto3" installed on the Ansible control host

### Lab Steps:

In the VS Code Explorer pane:

1. Right Click in the explorer pane
1. Select `New Directory`
1. Create a new directory named `template` and press enter
1. Right Click on the template folder in the explorer pane
1. Select 'New File'
1. Enter `config.j2` as the name:
1. Copy the following content to the file:

   ```
   # This is a configuration file generated from a Jinja2 template

   {% if config_type == "production" %}
   debug = false
   log_file = "/var/log/myapp.log"
   {% else %}
   debug = true
   log_file = "/tmp/myapp.log"
   {% endif %}
   ```

4. Right click and select New File to create a new file named "config.yml" and add the following content:

   ```
   ---
   config_type: "production"
   ```

5. Right Click and select New File to create a new playbook file named "generate_config.yml" and add the following content:

   ```
   - name: Generate config file
     hosts: windows
     tasks:
       - name: Generate config file from template
         template:
           src: templates/config.j2
           dest: C:\Users\Administrator\config.conf
         vars_files:
           - config.yml
   ```
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

## Run the Playbook

Run the "generate_config.yml" playbook against the "Windows Target 1" host using the following command:

   ```
   ansible-playbook generate_config.yml
   ```
Verify that the "config.conf" file was created on the "Windows Target 1" host with the correct content.
