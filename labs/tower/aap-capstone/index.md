# AAP - Capstone lab

## Objective

This is the final challenge where we try to put most of what you have learned together.



## Scenario

Your operations team and your application development team like what they see in Ansible Automation Platform. To really use it in their environment they put together these requirements:

* All webservers (`node1`, and `node2`) should go in one group

* As the webservers can be used for development purposes or in production, there has to be a way to flag them accordingly as `stage dev` or `stage prod`.

* Currently, `node1` is used as a development system and `node2` is in production.
  Of course, the content of the world-famous `index.html` will be different between `dev` and `prod` stages.
  * There should be a title on the page stating the environment
  * There should be a content field
* The content writer `wweb` should have access to a survey to change the content for `dev` and `prod` servers.

## Prepare your new 2 host

We need to configure credentials, inventory, etc for our new `centos2` host. 

Because `centos` and `centos` are part of the same inventory we need to use the same credentials for both. 

Log into `centos` and `cat ~/.ssh/centos.pub` and then add it to `centos2` `~/.ssh/authorized_keys`

After configuring the new host confirm the `ping` module works when executed against it. 

Add `centos2` to the following groups: 

* `web`
* `Webserver`

Make sure to choose `centos credential` for any ad-hoc commands or templates.



### Run Jobs

Run the following templates on `centos2`

* Install Apache 
* Create index.html 



## Project Code

All code is already in place. Check out the **Ansible Workshop Examples** git repository at https://github.com/jruels/workshop-examples. There you will find the playbook `webcontent.yml`, which calls the role `role_webcontent`.

Compared to the previous Apache installation role there is a major difference: there are now two versions of an `index.html` template, and a task deploying the template file which has a variable as part of the source file name:

`dev_index.html.j2`

```html
<body>
<h1>This is a development webserver, have fun!</h1>
{{ dev_content }}
</body>
```



`prod_index.html.j2`

```html
<body>
<h1>This is a production webserver, take care!</h1>
{{ prod_content }}
</body>
```



`main.yml`

```yaml
[...]
- name: Deploy index.html from template
  template:
    src: "{{ stage }}_index.html.j2"
    dest: /var/www/html/index.html
  notify: apache-restart
```



## Prepare Inventory

There is of course more than one way to accomplish this, but for the purposes of this lab, we will use Ansible automation controller.

Go to **Resources** -> **Inventories** and select **centos**.

Choose the **Groups** tab, click the **Add** button and create a new inventory group labeled `Webserver` and click **Save**.

In the **Details** tab of the `Webserver` group, click on **Edit**. Within the **Variables** textbox define a variable labeled `stage` with the value `dev` and click **Save**.

In the **Details** tab of the `Webserver` inventory, click the **Hosts** tab, click the **Add** button and **Add existing host**. Select `centos1`, and `centos2` as the hosts to be part of the `Webserver` inventory group.



In **Resources** -> **Inventories**, select the `centos` Inventory. Click on the `Hosts` tab and click on `centos2`. Click on `Edit` and add the `stage: prod` variable in the **Variables** window. This overrides the inventory variable due to the order of operations of how the variables are accessed during playbook execution.



## Create the Template

In **Resources** -> **Templates**, select the **Add** button and **Add job template** as follows:

| Parameter             | Value                                                        |
| --------------------- | ------------------------------------------------------------ |
| Name                  | Create Web Content                                           |
| Job Type              | Run                                                          |
| Inventory             | centos                                                       |
| Project               | Ansible Workshop Examples                                    |
| Execution Environment | AWX                                                          |
| Playbook              | rhel/apache/webcontent.yml                                   |
| Credentials           | centos credential                                            |
| Variables             | dev_content: "default dev content", prod_content: "default prod content" |
| Options               | Privilege Escalation                                         |

Click **Save**.

Run the template by clicking the **Launch** button.

### Check the Results

Run `curl http://localhost` against both hosts, `centos` and `centos2`. 



If everything worked as expected you should see something like: 

CentOS: 

```
<body>
<h1>This is a development webserver, have fun!</h1>
default dev content
</body>
```



CentOS2:

```
<body>
<h1>This is a production webserver, take care!</h1>
default prod content
</body>
```



## Add Survey

Add a Survey to the template to allow changing the variables `dev_content` and `prod_content`. 

In the Template, click the **Survey** tab and click the **Add** button. 

Fill out the following information:

| Parameter            | Value                                    |
| -------------------- | ---------------------------------------- |
| Question             | What should the value of dev_content be? |
| Answer Variable Name | dev_content                              |
| Answer Type          | Text                                     |

- Click **Save**
- Click the **Add** button

In the same fashion add a second **Survey Question**

| Parameter            | Value                                     |
| -------------------- | ----------------------------------------- |
| Question             | What should the value of prod_content be? |
| Answer Variable Name | prod_content                              |
| Answer Type          | Text                                      |

- Click **Save**

- Click the toggle to turn the Survey questions to **On**

  

  Add permissions to the team `Web Content` so the template **Create Web Content** can be executed by `wweb`.

- Within the **Resources** -> **Templates**, click **Create Web Content** and add **Access** to the user `wweb` the ability to execute the template.

  - **Select a Resource Type** -> click **Users**, click **Next**.
  - **Select Items from List** -> select the checkbox `wweb`, click **Next**.
  - **Select Roles to Apply** -> select the checkbox **Execute** and click **Save**.

- Run the survey as user `wweb`

  - Logout of the user `admin` of your Ansible automation controller.
  - Login as `wweb` and go to **Resources** -> **Templates** and run the **Create Web Content** template.

Check the results again, with `curl`, from your automation controller host.



You have done all the required configuration steps in the lab already. If unsure, refer back to the previous labs.



## Congrats! 



