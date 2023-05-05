## Lab Instructions: Running Ansible Playbook on a Windows Host

In this lab, you will learn how to use Ansible to manage Windows hosts using the WinRM protocol. You will use a sample playbook to perform some basic tasks on a Windows host.

### Prerequisites

1. An Ansible control node set up on a Linux machine with Ansible 2.10 or later installed.

2. A Windows host with WinRM configured and enabled. Ensure that the Ansible control node can communicate with the Windows host via WinRM.

3. A user account on the Windows host with administrative privileges. The user account should have a strong password, and you should have the credentials for the account.


### Setting up the Inventory File
Perform the following steps on Windows Target 1 in VS Code
In the VS Code Explorer pane:

1. Right Click in the explorer pane

2. Select `New File`

3. Enter the name and content details below:

4. Create a new file named `inventory_simple.yml` and open it in your preferred text editor.

5. Define the Windows host details in the inventory file in the following format:

   ```
   all:
     hosts:
       win-host:
         ansible_host: <win-host-ip>
         ansible_user: <win-user>
         ansible_password: <win-password>
         ansible_connection: winrm
         ansible_winrm_server_cert_validation: ignore
   ```
   Replace `<win-host-ip>` with the IP address of the Windows host, `<win-user>` with the administrative user account name, and `<win-password>` with the password for the user account.

   Note: If your Windows host is on a domain, you can use the `ansible_domain_username` and `ansible_domain_password` parameters instead of `ansible_user` and `ansible_password`.

### Running the Ansible Playbook
In the VS Code Explorer pane:

1. Right Click in the explorer pane
1.. Select `New Directory`
1. Create a new directory named `playbook-fun`
1. Right Click in the explorer pane
1. Select 'New File'
1. Enter `playbook.yml` as the name:
1. Copy the following YAML code to the `playbook.yml` file:

   ```
   ---
   - name: Playbook-Fun
     hosts: win-host
     gather_facts: no
     tasks:
     - name: Run PowerShell Command
       win_shell: |
         Write-Output "Hello World"
   ```
   This playbook contains a single task that runs a PowerShell command on the Windows host to display "Hello World" in the console.

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
5. Run the playbook using the following command:

## Run the Playbook

   ```
   ansible-playbook -i inventory.yml playbook.yml
   ```

   This command will run the `playbook.yml` playbook against the Windows host defined in the `inventory.yml` file.

   Verify that the playbook has run successfully by checking the output in the console. You should see "Hello World" displayed in the console.

Congratulations! You have successfully run an Ansible playbook on a Windows host.

### Conclusion

In this lab, you learned how to use Ansible to manage Windows hosts using the WinRM protocol. You used a sample playbook to perform some basic tasks on a Windows host. You also learned how to set up an inventory file with the Windows host details and run the playbook against the host.

Now that you have the basic understanding of how to use Ansible with Windows hosts, you can explore more advanced use cases and features to automate and manage your Windows infrastructure.
