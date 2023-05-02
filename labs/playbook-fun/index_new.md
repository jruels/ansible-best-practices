## Lab Instructions: Running Ansible Playbook on a Windows Host

In this lab, you will learn how to use Ansible to manage Windows hosts using the WinRM protocol. You will use a sample playbook to perform some basic tasks on a Windows host.

### Prerequisites

1. An Ansible control node set up on a Linux machine with Ansible 2.10 or later installed.

2. A Windows host with WinRM configured and enabled. Ensure that the Ansible control node can communicate with the Windows host via WinRM.

3. A user account on the Windows host with administrative privileges. The user account should have a strong password, and you should have the credentials for the account.

4. The `pywinrm` module installed on the Ansible control node. You can install it using the following command:
   ```
   pip3 install pywinrm
   ```

### Setting up the Inventory File

1. Create a new file named `inventory.yml` and open it in your preferred text editor.

2. Define the Windows host details in the inventory file in the following format:

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

3. Save the `inventory.yml` file.

### Running the Ansible Playbook

1. Create a new directory named `playbook-fun` and change to the directory.

2. Create a new file named `playbook.yml` in the `playbook-fun` directory and open it in your preferred text editor.

3. Copy the following YAML code to the `playbook.yml` file:

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

4. Save the `playbook.yml` file.

5. Run the playbook using the following command:
   ```
   ansible-playbook -i inventory.yml playbook.yml
   ```

   This command will run the `playbook.yml` playbook against the Windows host defined in the `inventory.yml` file.

6. Verify that the playbook has run successfully by checking the output in the console. You should see "Hello World" displayed in the console.

Congratulations! You have successfully run an Ansible playbook on a Windows host.

### Conclusion

In this lab, you learned how to use Ansible to manage Windows hosts using the WinRM protocol. You used a sample playbook to perform some basic tasks on a Windows host. You also learned how to set up an inventory file with the Windows host details and run the playbook against the host.

Now that you have the basic understanding of how to use Ansible with Windows hosts, you can explore more advanced use cases and features to automate and manage your Windows infrastructure.
