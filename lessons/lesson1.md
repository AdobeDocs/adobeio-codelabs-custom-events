## Lesson 1: Create a New Firefly App from Template 

### Create a Console Integration and Set up Project
In order to use custom event SDK, you need to get below informtion from console integration.

- `IMS Org Id`: The Organization Id in which the provider, event metadata, etc are to be created. 
- `API key`: The API Key ( client id ) for the integration ( project workspace ) 
- `JWT Token`: Note that I/O Management Service needs to be enabled for the integration
- `Config.zip`: Config file downloaded from console including private key and certificate_pub.crt
- `project.json`: for instance, `projectname-orgId-Production.json` file downloaded from console 


1. Navigate to Adobe I/O console at [https://console.adobe.io](https://console.adobe.io) in your browser and create a project or using your exsiting project 

2. Select `Add to Project` -> `Add an API` -> `Adobe Services` -> `I/O managemenet API`
![add-api](assets/add-api.png)

3. Follow the steps to configure API, create a new service account (JWT) credential, `config.zip` will be
downloaded automatically, you will need the private key to generate JWT token.

4. Go to `project overview` tab, download project metadata from below `Download` button and get the needed info from this `.json`file, or you can also get these info from `.aio` file in the project folder.

### Initialize a Firefly app using CLI template

To initialize a Firefly app, let's use init command from the CLI.

```bash
aio app init <Your-project-name>
```

You're presented with a few options, first question is "Which Adobe I/O App features do you want to enable for this project?" In this lab, we keep all of them. The second question is "Which type of sample actions do you want to create?" in this lab, we will select `Generic`, please remember to keep the `publish-event` template.
Name your action, then you have created your project firefly template. Now you can use this template to start your app

In this lab, I will create a webpage using this generic template. if this is your first time to use project firefly, follow below
* [Creating your First Firefly App](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md)

In our use case, we will develop a simple website with the `like` button. after you finished code development and deployed using `aio app deploy` you will be able to see your app like below
![webpage](assets/webpage.png)

In next lesson, we will show how to use custom event to register this app as event provider and click the like button 
will fire an event, this event will be consumed by three ways (Journaling, webhook URL, runtime action)

Next lesson: [Lesson2](lesson2.md)