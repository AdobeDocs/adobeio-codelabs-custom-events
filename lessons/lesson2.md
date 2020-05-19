## Lesson 2: Explore Custom Event SDK

Once you set up the project and credentials, below is a sample breakdown of how to use event SDK:
* Register and event provider
* Create event metadata
* Create jouranl registration or webhook registration 
* Fire event 

By completing these steps, you will be able to see your event provider and journal or webhook registration on developer console as show below

For your convenience,the project source code of the codelab is available [here](https://github.com/AdobeDocs/adobeio-codelabs-custom-event/tree/init-content/lessons/source/event-codelab-sample-code.zip)

### Register an event provider 
To register an event provider, sample code like this 
```javascript
async function createProvider(sdkClient) {
  // create a provider
  provider = await sdkClient.createProvider(consumerOrgId, projectId,
    workspaceId,
    {
      label: 'EventsSDKCodeLab ' + randomNumber,
      organization_id: orgId
    })
  return provider
}
```
### Create event metadata
sample code for create event metadata
```javascript
async function createEventMetadata(sdkClient, providerId) {
  // create event metadata
  eventCode = 'com.adobe.events.sdk.event.' + randomNumber
  return await sdkClient.createEventMetadataForProvider(consumerOrgId,
    projectId,
    workspaceId, providerId, {
      event_code: eventCode,
      description: 'EventsSDKCodeLab demo for Events SDK',
      label: 'EventsSDKCodeLab ' + randomNumber
    })
}
```

### Create journal registration and fetch jouranlling position
sample code for create journal registration
```javascript
async function registerJournallingEndpoint(sdkClient, providerId, eventCode) {
  // create journal registration
  journalReg = await sdkClient.createWebhookRegistration(consumerOrgId,
    integrationId, {
      name: 'EventsSDKCodeLab ' + randomNumber,
      description: 'EventsSDKCodeLab ' + randomNumber,
      client_id: apiKey,
      delivery_type: 'JOURNAL',
      events_of_interest: [
        {
          event_code: eventCode,
          provider_id: providerId
        }
      ]
    })
    return journalReg
}

async function fetchJournallingPosition(sdkClient, journallingUrl) {
  // fetch position for next event for journalling
  return await sdkClient.getEventsFromJournal(journallingUrl,
    { latest: true })
}
```
Note: there are two ways to consuming event: 1. through journaling 2. through webhook 
if you would like to use webhook, simply modify `delivery_type` and add `webhook_url` 
```javascript
{
      name: 'EventsSDKCodeLab ' + randomNumber,
      description: 'EventsSDKCodeLab' + randomNumber,
      client_id: apiKey,
      webhook_url: 'url',
      delivery_type: 'WEBHOOK',
      events_of_interest: [
        {
          event_code: eventCode,
          provider_id: providerId
        }
      ]
    }
```

### Fire event
```javascript
async function publishEvent(sdkClient, providerId, eventCode) {
  // fire event
  console.log(eventCode)
  console.log(providerId)
  const publish = await sdkClient.publishEvent({
    id: 'EventsSDKCodeLab-' + randomNumber,
    source: 'urn:uuid:' + providerId,
    time: ZonedDateTime.now(ZoneOffset.UTC).toString(),
    type: eventCode,
    data: {
      test: 'EventsSDKCodeLab_' + randomNumber
    }
  })
  console.log(publish)
}
```

### Sample code to execute 
```javascript
const run = async () => {
  let sdkClient = await initSDK()
  let provider = await createProvider(sdkClient)
  eventMetadata = await createEventMetadata(sdkClient, provider.id)
  journalReg = await registerJournallingEndpoint(sdkClient, provider.id, eventMetadata.event_code)
  await sleep(60000) //or remove sleep simple wait few secs for the regisration been created 
  journalling = await fetchJournallingPosition(sdkClient, journalReg.events_url)
  await publishEvent(sdkClient, provider.id, eventMetadata.event_code)
};
```

You could also delete event/registration/event providers using Event SDKs based on your need. 

### Check your result on Console
After using event SDK to create an event provider, you will see event provider registrated in console 
as well as your jouranling end point or webhook 
 ![event-provider](assets/event-provider.png)

Next lesson: [Ensure Events are published](lesson3.md)