# Lab Instructions for Running Ad-hoc Commands using Ansible on a Windows Host

In this lab, you will learn how to use Ansible to run ad-hoc commands against a Windows host. Ad-hoc commands are one-liners that you can use to perform specific tasks quickly and easily, without having to write a full playbook.

## Prerequisites

Before starting this lab, you should have the following:

- A Windows host that you can use as a target for Ansible commands
- A control node with Ansible installed

## Step 1: Run Ad-hoc commands
Run the following commands in the Ansible Control PuTTY window on Windows Target 1

1. Run the following ad-hoc command to check if the Windows host is reachable:

  ```bash
  ansible windows_host -i inventory_simple.yml -m win_ping
  ```

You should see a success message if the Windows host is reachable.

2. Run the following ad-hoc command to get information about the Windows host:

  ```bash
  ansible windows_host -i inventory_simple.yml -m setup
  ```

This command will retrieve various information about the Windows host, including hardware and software details, network settings, and more.

3. Run the following ad-hoc command to get a list of installed packages on the Windows host:

```bash
ansible windows_host -i inventory_simple.yml -m win_package -a "list=1"
```

This command will retrieve a list of all installed packages on the Windows host.

4. Run the following ad-hoc command to install a package on the Windows host:

```bash
ansible windows_host -i inventory_simple.yml -m win_package -a "name=<package_name> state=present"
```

Replace `<package_name>` with the name of the package you want to install.

5. Run the following ad-hoc command to uninstall a package from the Windows host:

```bash
ansible windows_host -i inventory_simple.yml -m win_package -a "name=<package_name> state=absent"
```

Replace `<package_name>` with the name of the package you want to uninstall.
