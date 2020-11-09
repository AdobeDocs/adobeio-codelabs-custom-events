## Lesson 3: Fire Event using Event SDK

### Fire Event
Once you set up the app and register the event provider, now you can make user click like button as fire event by adding code to your project firefly app

When you run below command 
```bash
aio app init
```
It provide you with the template of `publish event` to provide a way to convert an input json to cloud event and publish to events.
![event-provider](assets/publish-event-cli.png)

You can choose to use this template code at `/actions/publish-events/index.js` or create your own code.
Within the newly created app, Firstly, set up `package.json` with the lists of dependencies, version, etc. 
Then `manifest.yml` lists the declaration of serverless actions including name, source files, runtime kind, default params, annotations, and so on.

Note: here put in the `providerId` and `eventCode`from lesson 2 and `orgId`, `apiKey`, `accessToken`from console integration from lesson 2

Below is a sample `manifest.yml` 
```javascript
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions:
      event:
        function: actions/event/index.js
        web: 'yes'
        runtime: 'nodejs:10'
        inputs:
          LOG_LEVEL: debug
          orgId: <YOUR-ORG-ID>
          apiKey:  <YOUR-apiKey>
          accessToken: <YOUR-accessToken>
          providerId: <YOUR-providerId>
          eventCode: <YOUR-eventCode>
        annotations:
          final: true
```
The action to fire event is called `event` here, currently your app only has one action `event` :

* Source code is at `actions/event/index.js`
* It is a [web action](https://github.com/AdobeDocs/adobeio-runtime/blob/master/guides/creating_actions.md#invoking-web-actions)
* The action will be run in the `nodejs:10` [runtime container on I/O Runtime](https://github.com/AdobeDocs/adobeio-runtime/blob/master/reference/runtimes.md)
* It has some [default params](https://github.com/AdobeDocs/adobeio-runtime/blob/master/guides/creating_actions.md#working-with-parameters) such as `LOG_LEVEL`, `orgId`, `apiKey`, which are automatically available in the `params` object of the action without passing it to the action for every invocation. The `final` annotation set as `true` tells that those params are immutable.

Now let's have a deeper look at the action's source code.
Also one can find source code [here](https://github.com/AdobeDocs/adobeio-codelab-customevent-demo/blob/master/actions/event/index.js)

```javascript
/**
 * This action when user click the like button, fire an event
 */

const { Core } = require('@adobe/aio-sdk')
const fetch = require('node-fetch')
const ZonedDateTime = require('@js-joda/core').ZonedDateTime
const ZoneOffset = require('@js-joda/core').ZoneOffset
const EventsSDK = require('@adobe/aio-lib-events')

const httpOptions = { retries: 3 }

async function publishEvent(sdkClient, providerId, eventCode) {
  // fire event
  console.log(eventCode)
  console.log(providerId)
  const publish = await sdkClient.publishEvent({
    id: 'like-' + Math.round(Math.random() * 100000),
    source: 'urn:uuid:' + providerId,
    time: ZonedDateTime.now(ZoneOffset.UTC).toString(),
    type: eventCode,
    data: {
      text: 'you got one like'
    }
  })
  return publish
}

async function main (params) {
  // create a Logger
  const myAppLogger = Core.Logger('main', { level: params.LOG_LEVEL })
  // 'info' is the default level if not set
  myAppLogger.info('Calling the main action')

  // log levels are cumulative: 'debug' will include 'info' as well (levels are in order of verbosity: error, warn, info, verbose, debug, silly)
  myAppLogger.debug(`params: ${JSON.stringify(params, null, 2)}`) // careful to not log any secrets!

  try {
    let sdkClient = await EventsSDK.init(params.orgId, params.apiKey, params.accessToken, httpOptions)

    event_ret = await publishEvent(sdkClient, params.providerId, params.eventCode)
    var returnObject = {
      statusCode: 200,
      headers: {
        'Content-Type': 'application/json'
      },
      body: event_ret
    };

    return returnObject
  } catch (error) {
    myAppLogger.error(error)
    return {
      statusCode: 500,
      body: { error: 'server error!' }
    }
  }
}

exports.main = main
```
What happens here is that the action exposes a `main` function, which accepts a list of params from the client. It checks the required params for using the `EventsSDK`. 

Next, let's see how the web UI communicates with the backend. All web assets are placed in the web-src folder.
Beside a few auto-generated files that are useful for running your app on Adobe Experience Cloud (ExC) Shell, `App.js` is the extension point of your UI.
By default, it contains a form that lists all available backend actions, allows you to select the action to be invoke.
```javascript
render () {
  return (
    // ErrorBoundary wraps child components to handle eventual rendering errors
    <ErrorBoundary onError={this.onError} FallbackComponent={this.fallbackComponent} >
    <section class="banner style1 orient-left content-align-left image-position-right fullscreen onload-image-fade-in onload-content-fade-right">
        <div class="content">
            <h1>Basel</h1>
              <p class="major">A city in northwestern Switzerland on the river Rhine. Basel is Switzerland's third-most-populous city. The city is known for its many internationally renowned museums, ranging from the Kunstmuseum, the first collection of art accessible to the public in Europe (1661) and the largest museum of art in the whole of Switzerland.</p>
              <ul class="action">
                <li><a href={actions.event}>Like</a></li>
              </ul>
        </div>
        <div class="image">
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f5/Basel_-_M%C3%BCnsterpfalz1.jpg/2560px-Basel_-_M%C3%BCnsterpfalz1.jpg" alt="" />
        </div>
      </section>
    </ErrorBoundary>
  )
  }
}

```
source code is [here](https://github.com/AdobeDocs/adobeio-samples-custom-events/blob/master/web-src/src/App.js)

### Run and Deploy the Firefly App
You can run the firefly app locally by execute the below command with AIO CLI
```bash
aio app run
```
This command will deploy the `event` action into I/O Runtime, and spins up a local instance for the UI. When the app is up and running, it can be seen at `https://localhost:9080`. You should be able to see the UI of the app and it is also possible to access the app from ExC Shell: `https://experience.adobe.com/?devMode=true#/apps/?localDevUrl=https://localhost:9080`. You might be asked to log in using your Adobe ID.  When the website is opened, the UI is almost similar to what you see when deployed on localhost, except the ExC Shell on top of the UI.

Once you are satisfied with your app, you can deploy your app using the `namespace` and `auth` you get from console integration, execute the below command with AIO CLI
```bash
aio app deploy
```
This command will deploy the app to your namespace, you will get the URL like 
`https://<Runtime-namespace>.adobeio-static.net/<project-name>-0.0.1/index.html`
Now click the `like` button, you should be able to fire the event 

For your reference, the solution code for lesson 3 and lesson 4 is [here](https://github.com/AdobeDocs/adobeio-samples-custom-events) 

Next lesson: [Lesson4](lesson4.md)