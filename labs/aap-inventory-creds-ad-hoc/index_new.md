# Lab Title: Using AWX with AAP to Manage Windows Hosts

Lab Objective: In this lab, you will learn how to use AWX with AAP to manage Windows hosts. You will configure AWX with the necessary credentials to allow it to authenticate against a Windows host using AAP, and then you will run an ad-hoc command against the Windows host to verify the connection.

Lab Steps:

1. In Chrome navigate to the public IP of your Ansible AWX Server
2. In the "ansible-working" repository on the Windows Target 1 host using the Source Control pane in Visual Studio Code.
3. In the cloned repository, create a new directory named "playbooks".
4. In the "playbooks" directory, create a new file named "ad_hoc.yml" and add the following content:

```
- name: Run ad-hoc command
  hosts: windows
  gather_facts: no
  tasks:
    - name: Execute command
      win_command: whoami
```

5. In AWX, navigate to the "Credentials" section and add a new "Machine" credential for the Windows Target 1 host using the following settings:
   * Credential Type: "Windows"
   * Username: "Administrator"
   * Password: "JustM300"
   * Host: "<IP address of Windows Target 1>"
   * Port: "5985"
   * Connection: "winrm"
   * Transport: "ntlm"

6. In AWX, navigate to the "Inventories" section and add a new inventory named "AAP Inventory".
7. In the "AAP Inventory", create a new host named "Windows Target 1" with the following settings:
    * Variables:
        ```
        ansible_connection: winrm
        ansible_winrm_server_cert_validation: ignore
        ansible_user: Administrator
        ansible_password: "{{ vault_aap_password }}"
        ```
    * Credentials:
        * Machine: "Windows Target 1"
8. In AWX, navigate to the "Templates" section and add a new template named "Run ad-hoc command".
9. In the "Run ad-hoc command" template, select the "AAP Inventory" as the inventory, and select the "ad_hoc.yml" playbook as the playbook.
10. Save all changes to the repository and commit them using the Source Control pane in Visual Studio Code.
11. Push the changes to the "ansible-working" repository on GitHub using the Source Control pane in Visual Studio Code.
12. Switch to the Ansible control host and navigate to the directory where the "ansible-working" repository was cloned.
13. Use the "git pull" command to update the cloned repository with the latest changes.
14. In AWX, navigate to the "Templates" section and click the "Run" button next to the "Run ad-hoc command" template.
15. Verify that the ad-hoc command was executed successfully on the "Windows Target 1" host.
