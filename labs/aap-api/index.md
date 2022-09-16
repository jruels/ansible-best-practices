# AAP - REST API

## Objective

In this lab we’ll describe two ways to discover the Automation Platform API if you need to dive in deeper. While the [principles of the AAP API](https://docs.ansible.com/ansible-tower/latest/html/towerapi/index.html) are documented and there is an [API reference guide](https://docs.ansible.com/ansible-tower/latest/html/towerapi/api_ref.html#/), it’s often more efficient to just browse and discover the API.



## Browsing and Using the AAP API interactively

The AAP API is browsable, which means you can just click your way through it:

1. Go to the AAP UI in your browser and make sure you’re logged in as admin.
2. Replace the end of the URL with `/api` e.g. `https://<TOWER LAB VM>/api`
3. There is currently only one API valid, so while in `/api/v2`:
   - you see a list of clickable object types
   - on the right upper side, there is a button **OPTIONS** which tells you what you can do with the current object in terms of API.
   - next to it there is a **GET** button which allows you to choose between getting the (raw or not) JSON output or the API format, which you’re currently admiring by default.
4. Click on the `/api/v2/users/` link and discover some more features:
   - There is a list of all objects of the given type
   - Each individual object can be reached using the `url` field (`url`: `/api/v2/users/1`)
   - Most objects have a `related` field, which allows you to jump from object to object
   - At the bottom of the page, there is a new field that allows you to **POST** a new object, so let’s do this and create a new user name John Smith (user name doesn’t matter)



Try to figure this out on your own. 

The instructor will show the solution at the end of the lab time. 



Now log in again as admin and go back to the list of users: `https://<TOWER LAB VM>/api/v2/users/`

- Click on the **url** field of your new friend John Smith and notice a few more things:
  - There is a red **DELETE** button at the top right level. Guess for what?
  - At the bottom of the page, the dialog shows **PUT** and **PATCH** buttons.

Patch the user to be named “Johnny” instead of “John”?



Try to figure this out on your own. 

The instructor will show the solution at the end of the lab time. 



Change the **last_name** to “Smithy” using the **PUT** verb. What happens?



When you’re done press the red **DELETE** button and remove Johnny Smithy.

## Challenge Lab: Create inventory
For this challenge, use the api to create a new inventory named `api inventory`, that includes the host `Server2`.


## Congrats!

