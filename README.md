# Build Event-Driven Firefly App Using Custom Events

### Overview

[Project Firefly](https://github.com/AdobeDocs/project-firefly) is a complete framework that enables enterprise developers to build and deploy custom web applications that extend Adobe Experience Cloud solutions and run on Adobe infrastructure. It leverages modern technologies (JAM stack, serverless computing, Node, and React) and ensures best practices when building applications (event-driven architecture, microservices, continuous integration, and delivery). 

[Custom Event CLI Plugin](https://github.com/adobe/aio-cli-plugin-events) is an open-source events plugin for the use of third party customers as part of Project firefly. Adobe I/O custom events allows you to build reactive, event-driven applications, based on events originating from various Adobe services. Events are triggered by event providers and can be listened to by journalling or by registering a webhook, Adobe I/O also provide the [Custom Event SDK](https://github.com/adobe/aio-lib-events). The events SDK provides a wrapper over these API calls making it easier for developers to use it as part of their apps. 

### User Story
There are times when users may wish to register their own apps as event provider and integrate other technologies in an event-driven manner (e.g. with other 3rd-party ESPs, notifying call-centres, conducting look-ups to CRM platforms such as Dynamics prior to an e-mail send, etc) 
A specific demonstration to illustrate this use case has therefore been put together in this lab:
* Jane build an app using project firefly and she would like to track the likes been clicked by reader
* Jane registered her app as event provider using custom event
* Joe could subscribed events from Jane's custom app event registration 
* When reader click like button or leave a comment, Jane and Joe will all receive event notification

In this lab, you will learn how to:
* Build a simple web app using project firefly framework 
* Enable event-driven workflows using Adobe I/O custom event template 

Next: [Requirements](/lessons/requirements.md).


