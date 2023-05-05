## Lab Title: Using Templates to Create Configuration Files

### Lab Objective:
In this lab, you will learn how to use Ansible's template module to create configuration files based on a template file and variables. You will create a template file and use it to generate a configuration file on a Windows host.
Sure thing! Here's a lab guide on how to create an Ansible template and playbook to configure a Windows IIS web.config file:

## Prerequisites

Before you begin, you'll need:

- A Windows Server 2022 workstation with Git, Visual Studio Code, and PuTTY preinstalled.
- A GitHub account.
- Basic knowledge of Git and GitHub.
- Basic knowledge of IIS and web.config files.
- A basic understanding of Ansible.

## Step 1: Clone the repository

If you have not already done so, you need to clone the ansible-working repository to your Windows Server 2022 workstation. Follow these steps:

1. Open Git Bash or your preferred terminal application.
2. Navigate to the directory where you want to store your repository.
3. Run the following command to clone the repository:

```
git clone https://github.com/your-username/ansible-working.git
```

Note: Replace "your-username" with your GitHub username.

## Step 2: Create the Ansible playbook

Now it's time to create the Ansible playbook that will use the template to configure the web.config file. Follow these steps:

1. Open Visual Studio Code.
2. Click the "File" menu and choose "Open Folder".
3. Navigate to the directory where you cloned your repository in Step 2 and click "Open".
4. Click the "Explorer" icon on the left sidebar.
5. Right-click on the root folder of your project and choose "New File".
6. Name the file `configure.yml`.
7. Copy and paste the following code into `configure.yml`:

```
- name: Configure web.config
  hosts: all
  tasks:
    - name: Create web.config file
      template:
        src: templates/web.config.j2
        dest: C:\inetpub\wwwroot\web.config
```

This playbook will use the `template` module to generate the web.config file from a Jinja2 template.

## Step 3: Create the Ansible template

Next, you need to create the Jinja2 template that will be used to generate the web.config file. Follow these steps:

1. In Visual Studio Code, right-click on the `templates` folder in the root directory of your project.
2. Choose "New File".
3. Name the file `web.config.j2`.
4. Copy and paste the following code into `web.config.j2`:

```
<?xml version="1.0"?>
<configuration>
    <system.webServer>
        <rewrite>
            <rules>
                {% if redirect_to_www %}
                <rule name="Redirect to www" stopProcessing="true">
                    <match url=".*" />
                    <conditions>
                        <add input="{HTTP_HOST}" pattern="^(?!www)(.*)$" />
                    </conditions>
                    <action type="Redirect" url="http://www.{HTTP_HOST}{REQUEST_URI}" redirectType="Permanent" />
                </rule>
                {% endif %}
                {% if redirect_to_https %}
                <rule name="Redirect to HTTPS" stopProcessing="true">
                    <match url=".*" />
                    <conditions>
                        <add input="{HTTPS}" pattern="off" />
                    </conditions>
                    <action type="Redirect" url="https://{HTTP_HOST}{REQUEST_URI}" redirectType="Permanent" />
                </rule>
                {% endif %}
                {% if enable_compression %}
                <rule name="Compress static files" enabled="true">
                    <match url=".*\.(js|css|html|htm|png|jpg|jpeg|gif|bmp|ico|xml|txt|svg|json)$" />
                    <action type="Rewrite" url="gzip.aspx?url={R:0}" />
                    <conditions>
                        <add input="{HTTP_ACCEPT_ENCODING}" pattern="gzip" />
                        <add input="{QUERY_STRING}" pattern="no-compression" negate="true" />
                    </conditions>
                </rule>
                {% endif %}
            </rules>
        </rewrite>
        <httpErrors errorMode="Detailed" />
        <security>
            <requestFiltering allowDoubleEscaping="true" />
            <ipSecurity allowUnlisted="false">
                <add ipAddress="127.0.0.1" allowed="true" />
            </ipSecurity>
        </security>
    </system.webServer>
</configuration>
```

This template includes several configuration options that you can customize for your specific needs, such as:

- Redirecting HTTP requests to HTTPS.
- Redirecting requests to the www subdomain.
- Enabling static file compression.

## Step 4: Run the playbook

Now that you've created the playbook and template, it's time to run the playbook to configure the web.config file. Follow these steps:

1. Open a PuTTY session and connect to your Ansible control host.
2. Navigate to the directory where you cloned your repository in Step 2.
3. Run the following command to pull the latest changes from the GitHub repository:

```
git pull
```

4. Run the following command to run the playbook:

```
ansible-playbook configure.yml
```

This command will execute the `configure.yml` playbook, which will generate the `web.config` file using the `web.config.j2` template and place it in the `C:\inetpub\wwwroot` directory on all hosts in the `all` group.

## Conclusion

In this lab, you learned how to create an Ansible playbook and template to configure a Windows IIS web.config file. You created a GitHub repository to store your playbook and template, used Visual Studio Code to edit your files, and used PuTTY to pull your changes to your Ansible control host and run the playbook. By following these steps, you can easily automate the configuration of your web.config file across multiple hosts, saving you time and effort.
