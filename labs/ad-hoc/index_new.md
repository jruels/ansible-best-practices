# Lab Instructions for Running Ad-hoc Commands using Ansible on a Windows Host

In this lab, you will learn how to use Ansible to run ad-hoc commands against a Windows host. Ad-hoc commands are one-liners that you can use to perform specific tasks quickly and easily, without having to write a full playbook.

## Prerequisites

Before starting this lab, you should have the following:

- A Windows host that you can use as a target for Ansible commands
- A control node with Ansible installed

## Step 1: Set up the inventory file
Perform these steps in VS Code on Windows Target 1

1. Create an inventory file named `windows_hosts.yml` in your working directory using the following YAML format:
  In the VS Code Explorer pane

    1. Right Click in the explporer pane
    1. Select `New File`
    1. Name the new file 'windows_hosts.yml
    1. Paste the code below into the file

```yaml
all:
  hosts:
    windows_host:
      ansible_host: <IP address or hostname of the Windows host>
      ansible_user: <username with administrative privileges on the Windows host>
      ansible_password: <password for the above user>
      ansible_connection: winrm
      ansible_winrm_server_cert_validation: ignore
```

Make sure to replace `<IP address or hostname of the Windows host>`, `<username with administrative privileges on the Windows host>`, and `<password for the above user>` with the appropriate values for your Windows host.

2. Save, Commit and Sync the `windows_hosts.yml` file with GitHub in the VS Code Source Control Pane.

## Step 2: Run Ad-hoc commands
In the Ansible Control PuTTY window

2. Update the local files with a Pull
```
git pull
```
4. Run the following ad-hoc command to check if the Windows host is reachable:

```bash
ansible windows_host -i windows_hosts.yml -m win_ping
```

You should see a success message if the Windows host is reachable.

3. Run the following ad-hoc command to get information about the Windows host:

```bash
ansible windows_host -i windows_hosts.yml -m setup
```

This command will retrieve various information about the Windows host, including hardware and software details, network settings, and more.

4. Run the following ad-hoc command to get a list of installed packages on the Windows host:

```bash
ansible windows_host -i windows_hosts.yml -m win_package -a "list=1"
```

This command will retrieve a list of all installed packages on the Windows host.

5. Run the following ad-hoc command to install a package on the Windows host:

```bash
ansible windows_host -i windows_hosts.yml -m win_package -a "name=<package_name> state=present"
```

Replace `<package_name>` with the name of the package you want to install.

6. Run the following ad-hoc command to uninstall a package from the Windows host:

```bash
ansible windows_host -i windows_hosts.yml -m win_package -a "name=<package_name> state=absent"
```

Replace `<package_name>` with the name of the package you want to uninstall.
