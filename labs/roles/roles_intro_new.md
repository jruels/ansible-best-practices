# Lab Title: Creating and Using Ansible Roles

Lab Objective: In this lab, you will learn how to create and use Ansible roles to organize your playbook code. You will create a simple role that installs and configures Nginx on a Windows host.
Perform the lab steps on Windows Target 1 in VS Code

Lab Steps:

1. Using the Source Control pane in Visual Studio Code.
2. In the cloned "ansible-working" repository, create a new directory named "roles".
3. Change the current working directory to the "roles" directory.
4. Create a new role using the "ansible-galaxy" command by running the following command:

   ```
   ansible-galaxy init nginx
   ```

5. In the "tasks" subdirectory of the "nginx" role, create a new file named "main.yml" and add the following content:

   ```
   ---
   - name: Install Nginx
     win_chocolatey:
       name: nginx
       state: present

   - name: Start Nginx service
     win_service:
       name: nginx
       state: started
   ```

6. In the "handlers" subdirectory of the "nginx" role, create a new file named "main.yml" and add the following content:

   ```
   ---
   - name: restart nginx
     win_service:
       name: nginx
       state: restarted
   ```

7. In the root directory of the "ansible-working" repository, create a new file named "playbook.yml" and add the following content:

    ```
    ---
    - name: Install and configure Nginx using a role
      hosts: windows
      roles:
        - nginx
    ```

8. Save all changes to the repository and commit them using the Source Control pane in Visual Studio Code.
9. Push the changes to the "ansible-working" repository on GitHub using the Source Control pane in Visual Studio Code.
10. Switch to the Ansible control host and navigate to the directory where the "ansible-working" repository was cloned.
11. Use the "git pull" command to update the cloned repository with the latest changes.
12. Run the "playbook.yml" playbook against the "Windows Target 1" host using the following command:

    ```
    ansible-playbook playbook.yml
    ```

13. Verify that Nginx is installed and running on the "Windows Target 1" host.
