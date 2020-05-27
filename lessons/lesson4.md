## Lesson 4: Ensure Events are published

There are three ways one can consume event
* Using Journaling API 
* Using runtime action 
* Using webhook URL


### Option 1: Using Journaling API to consume events 
For enterprise developers, Adobe offers another way to consume events besides webhooks: journaling. The Adobe I/O Events Journaling API enables enterprise integrations to consume events according to their own cadence and process them in bulk. Unlike webhooks, no additional registration or other configuration is required; every enterprise integration that is registered for events is automatically enabled for journaling. Journaling data is retained for 7 days 

After you fire event, you should be able to verify your event through journaling `UNIQUE API ENDPOINT` you get from console by follow below instruction
[Journaling api](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/intro/journaling_api.md)
You could use `Curl` command or `POSTMAN` to call this journaling `UNIQUE API ENDPOINT` to see your fired event.
Or you can use [Custom event SDK](https://github.com/adobe/aio-lib-events/) to calling Journaling API to retrieve your event


### Option 2: Using runtime action
Once you have access to [Adobe I/O Runtime](https://www.adobe.io/apis/cloudplatform/runtime.html) (In our case you already have) and you have your [slack webhook url defined](https://api.slack.com/incoming-webhooks) :
* Edit the `manifest.yml` to add an action called slack 
```javascript
 slack:
        function: actions/slack/index.js
        web: 'yes'
        runtime: 'nodejs:10'
        inputs:
          LOG_LEVEL: debug
        annotations:
          final: true
```
Add in the actions folder with `actions/slack/index.js` with below sample code

```javascript
 var request = require('request');

/* default slackwebhook and channel add yours here and replace the TODO below */
var slackWebhook = "<your-slack-webhook>;
var slackChannel = "<your-channel>";

async function main (params) {
  
  /* print event detail */
  console.log('in main + event detail: ', params.event);

  var returnObject = {
    statusCode: 200,
    headers: {
      'Content-Type': 'application/json'
    },
    body: ""
  };

  /* handle the challenge */
  if (params.challenge) {

    console.log('Returning challenge: ' + params.challenge);

    returnObject.body = new Buffer(JSON.stringify({
      "challenge": params.challenge
    })).toString('base64');

    return returnObject;

  } else {

    /* we need it to run asynchronously, so we are returning a Promise */
    return new Promise(function (resolve, reject) {

      var slackMessage = " Event received: " + JSON.stringify(params);

      var payload = {
        "channel": slackChannel,
        "username": "incoming-webhook",
        "text": slackMessage,
        "mrkdwn": true,
      };

      var options = {
        method: 'POST',
        url: slackWebhook,
        headers:
            { 'Content-type': 'application/json' },
        body: JSON.stringify(payload)
      };

      request(options, function (error, response, body) {
        if (error) {

          console.log("ERROR: fail to post " + response);

          reject(error);

        } else {

          console.log ("SUCCESS: posted to slack " + slackMessage);

          returnObject.body = new Buffer(JSON.stringify({
            "slackMessage": slackMessage
          })).toString('base64');

          resolve(returnObject);
        }

      });

    });

  }
}

exports.main = main
```
After you deployed your runtime action, you can verfiy the webhook is working by 
```bash
curl -X POST -H 'Content-type: application/json' --data '{"text":"Hello, World!"}' https://<your-namespace>.adobeio-static.net/api/v1/web/event-demo-0.0.1/slack
```
In addtion, in developer console, you will be able to see ![slack webhook](assets/slack-webhook.png)
Select the slack one and save it. Now when you fire event, you should be able to receive a slack message every time people click the like button 
![slack message](assets/slack-message.png)

### Option 3: Using webhook to consume events 
You could configure another event delivery method through console by `Edit Events Registration` and add webhook or by using event SDK
More details about webhook, 
![webhook](assets/webhook.png)

Before you can register a webhook, the webhook needs to be online and operational. If not, then the registration will fail. So you need to take care of setting that up first. Your webhook must be hosted on a server. For development, you may use localhost along with a tool like [Ngrok](https://ngrok.com/).

Your webhook needs to
-   be accessible from the internet (localhost won't work)
-   be reachable over HTTPS
-   correctly respond to a "challenge" request
For more details, follow the link below: 
[how to use webhook](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/intro/webhook_docs_intro.md)

### Let's test it, and fire events
With that,once you fire the event (in our codelab case, click the like button) you should see them appearing in above three options:
- Through Adobe I/O Journaling API - in our demo case show up in webpage by using runtime action calling journaling API
- Get slack message - through slack runtime action webhook
- Through webhook URL

Next: [Well done](/lessons/welldone.md)