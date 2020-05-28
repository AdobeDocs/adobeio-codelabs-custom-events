## Lesson 1: Create a New Firefly App from Template 

To initialize a Firefly app, let's use init command from the CLI.

```bash
aio app init <Your-project-name>
```

You're presented with a few options Which Adobe I/O App features do you want to enable for this project? In this lab, we keep all of them.
The second question is Which type of sample actions do you want to create? in this lab, we will select `Generic` .
Name your action, then you have created your project firefly template. Now you can use this template to start your app

In this lab, I will create a webpage using this generic template. if this is your first time to use project firefly, follow below
* [Creating your First Firefly App](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md)

To simplified the lab, and focus on using custom event, the project firefly app source code is [here](https://github.com/AdobeDocs/adobeio-codelab-customevent-demo)

After you finished code development and deployed using `aio app deploy` you will be able to see your app like below
![webpage](assets/webpage.png)

In next lesson, we will show how to use custom event to register this app as event provider and click the like button 
will fire an event, this event will be consumed by three ways (Journaling, webhook URL, runtime action)

Next lesson: [Lesson2](lesson2.md)