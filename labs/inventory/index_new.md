# Ansible Best Practices for Windows - Inventory Lab Exercise

## Objective

The objective of this lab exercise is to familiarize yourself with the best practices of using Ansible for Windows and to apply the following:

- Use of [win_ping module](https://docs.ansible.com/ansible/latest/collections/ansible/windows/win_ping_module.html) to ensure connectivity between the Ansible control node and the Windows host
- Use of [win_feature module](https://docs.ansible.com/ansible/latest/collections/ansible/windows/win_feature_module.html) to install or remove Windows features
- Use of [win_iis_website module](https://docs.ansible.com/ansible/latest/collections/ansible/windows/win_iis_website_module.html) to manage IIS websites and application pools

## Lab Environment

This lab requires the following:

- A Windows Server 2019 or Windows 10 host with IIS installed
- An Ansible control node with the following:
  - Ansible 2.9 or later installed
  - Access to the Windows host via WinRM

## Lab Instructions
Sure, here are the instructions in Github Markdown format, including the YAML formatted inventory file, and additional details for installing Ansible using pip3:

## Provisioning an IIS Server using Ansible

These instructions will guide you through the process of provisioning an IIS (Internet Information Services) server using Ansible on a Windows host.

### Prerequisites

- A Windows host running IIS
- Python 3 installed on the Windows host
- Pip3 installed on the Windows host

### Installing Ansible using pip3

1. Open a PowerShell terminal as an Administrator.
2. Run the following command to upgrade pip: 

    ```powershell
    python -m pip install --upgrade pip
    ```

3. Run the following command to install Ansible using pip3:

    ```powershell
    pip3 install ansible
    ```

### Inventory File

Create an inventory file with the following content:

```yml
all:
  hosts:
    <IP or hostname of the IIS server>:
      ansible_connection: winrm
      ansible_winrm_transport: ntlm
      ansible_winrm_server_cert_validation: ignore
  vars:
    ansible_user: <username with administrative privileges>
    ansible_password: <password for the username>
```

Replace `<IP or hostname of the IIS server>` with the IP address or hostname of your IIS server, and replace `<username with administrative privileges>` and `<password for the username>` with the username and password of an account with administrative privileges on the IIS server.

### Running Ad-hoc Commands

To run ad-hoc commands on the IIS server, use the following command:

```bash
ansible all -i <path to inventory file> -m <module name> -a "<module arguments>"
```

Replace `<path to inventory file>` with the path to your inventory file, `<module name>` with the name of the Ansible module you want to use (e.g. `win_command`), and `<module arguments>` with the arguments for the module.

For example, to run the `win_command` module to execute the `dir` command on the IIS server, use the following command:

```bash
ansible all -i inventory.yml -m win_command -a "cmd='dir'"
```

### Running Playbooks

To run a playbook on the IIS server, create a YAML file with the playbook content, and run the following command:

```bash
ansible-playbook <path to playbook YAML file> -i <path to inventory file>
```

Replace `<path to playbook YAML file>` with the path to your playbook YAML file, and `<path to inventory file>` with the path to your inventory file.

For example, to create a playbook that installs IIS using the `win_feature` module, create a YAML file with the following content:

```yml
- name: Install IIS
  hosts: all
  tasks:
    - name: Install IIS
      win_feature:
        name: Web-Server
        state: present
```

Save the file as `install_iis.yml`, and run the following command to execute the playbook:

```bash
ansible-playbook install_iis.yml -i inventory.yml
```

This will install IIS on the target server.

### Conclusion

By following these instructions, you should now have an IIS server provisioned and managed by Ansible on a Windows host.
