## Lesson 3: Ensure Events are published

### Using Journaling API to consume events 
For enterprise developers, Adobe offers another way to consume events besides webhooks: journaling. The Adobe I/O Events Journaling API enables enterprise integrations to consume events according to their own cadence and process them in bulk. Unlike webhooks, no additional registration or other configuration is required; every enterprise integration that is registered for events is automatically enabled for journaling. Journaling data is retained for 7 days 

After you fire event, you should be able to verify your event through journaling `UNIQUE API ENDPOINT` you get from console by follow below instruction
[Journaling api](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/intro/journaling_api.md)
You could use `Curl` command or `POSTMAN` to call this journaling `UNIQUE API ENDPOINT` to see your fired event

### Using webhook to consume events 
You could configure another event delivery method through console by `Edit Events Registration` and add webhook or by using event SDK
More details about webhook, follow the link below: 
[how to use webhook](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/intro/webhook_docs_intro.md)
![webhook](assets/webhook.png)

Next: [Well done](/lessons/welldone.md)