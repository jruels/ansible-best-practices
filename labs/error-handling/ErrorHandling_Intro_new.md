## Error Handling - Introduction

In this lab, we will explore how to handle errors that may occur while running Ansible playbooks. Specifically, we will focus on how to handle errors related to Windows modules.

### Prerequisites

To complete this lab, you will need:
- An Ansible control node running on a Linux machine
- Visual Studio Code installed on your local machine
- An installed Git client
- Access to an internet connection

### Step 1: Clone the Ansible-worker Repository
If you have not done so already

1. Open Visual Studio Code.
2. In the sidebar, click on the "Source Control" icon (it looks like a branch).
3. In the "Source Control" pane, click on the "Clone Repository" button.
4. In the "Clone Repository" dialog, paste the following URL into the "Repository URL" field: https://github.com/<your-github-username>/ansible-worker.git
5. Choose a location to clone the repository to on your local machine.
6. Click "Clone" to clone the repository.

### Step 2: Edit the Error Handling Playbook

1. In Visual Studio Code, open the `error_handling.yml` playbook file located in the `labs/error-handling` directory of the ansible-best-practices-windows repository.
2. Modify the playbook to add error handling for the `win_service` module. Specifically, add a `register` variable to capture the output of the `win_service` module, and add a `failed_when` clause to check for errors.
3. Save the modified playbook to the `ansible-working` repository (c:\GitRepos\ansible-working).

### Step 3: Commit and Push Changes to GitHub

1. In the sidebar, click on the "Source Control" icon (it looks like a branch).
2. In the "Source Control" pane, review the changes you made to the playbook file.
3. Enter a commit message that describes the changes you made.
4. Click the checkmark icon to commit the changes.
5. Click on the "..." menu in the "Source Control" pane, and select "Push" to push the changes to GitHub.

### Step 4: Update the Ansible Control Host

1. Connect to your Ansible control host.
2. Navigate to the directory where you cloned the `ansible-worker` repository.
3. Run `git pull` to update the repository on the control host.

### Step 5: Execute the Error Handling Playbook

1. In the terminal on the Ansible control host, navigate to the `labs/error-handling` directory of the `ansible-worker` repository.
2. Run the following command to execute the modified `error_handling.yml` playbook against the Windows hosts:

   ```
   ansible-playbook -i inventory.yml error_handling.yml
   ```

   This will execute the playbook and handle any errors that occur.

Congratulations! You have successfully edited an Ansible playbook to handle errors, committed and pushed changes to GitHub, and updated the Ansible control host to execute the modified playbook.
