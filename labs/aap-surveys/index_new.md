# Lab Instructions for Ansible Playbook with Surveys

## Introduction

In this lab, you will create an Ansible Playbook with surveys. Surveys allow you to prompt users for additional variables when running a playbook. This is useful when you need to gather information that is not readily available or cannot be statically defined. 

## Prerequisites

Before you begin, ensure that the following requirements are met:

- You have access to Windows Target 1 and Ansible Control.
- Visual Studio Code and git are installed on Windows Target 1.
- You have a git repository cloned from GitHub called 'ansible-working' on Ansible Control.

## Steps

Follow the steps below to create the Ansible Playbook with surveys:

1. Open Visual Studio Code on Windows Target 1.
2. Create a new file and name it `playbook.yml`.
3. Copy and paste the following playbook into the `playbook.yml` file:
   ```
   ---
   - name: Playbook with survey
     hosts: all
     gather_facts: false
     vars_prompt:
       - name: privatePrompt
         prompt: "Private or Hidden Prompts are the default in Ansible Surveys. Type Anything"
       - name: publicPrompt
         prompt: "With the private: no setting you can see your response to the Survey Prompt. Type Anything"
         private: no
     tasks:
       - name: Display public and private responses
         debug:
           msg: "This is your response to the private prompt {{ privatePrompt }}. This is the public resposne {{ publicPrompt }}."
   ```
   This playbook prompts the user for two variables using surveys and then displays the responses.
4. Save the `playbook.yml` file and exit Visual Studio Code.
5. Open a PuTTY connection to Ansible Control.
6. Change the directory to the `ansible-working` repository using the following command:
   ```
   cd ansible-working
   ```
7. Run the following command to pull the changes made to the repository:
   ```
   git pull
   ```
8. Run the playbook using the following command:
   ```
   ansible-playbook playbook.yml
   ```
9. Follow the prompts and enter your responses.
10. After the playbook finishes running, verify that the responses were displayed correctly.

## Conclusion

In this lab, you created an Ansible Playbook with surveys, committed it to a git repository, and ran it using the Ansible Control host. You now know how to use surveys in Ansible Playbooks to gather additional information from users.



