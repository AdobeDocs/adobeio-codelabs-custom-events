## Lesson 3: Explore Custom Event SDK

### Register an event provider 
To register an event provider, sample code like this 
```javascript
async function createProvider(sdkClient) {
  // create a provider
  provider = await sdkClient.createProvider(consumerOrgId, projectId,
    workspaceId,
    {
      label: 'EventsSDKE2E ' + randomNumber,
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
      description: 'E2E test for Events SDK',
      label: 'EventsSDKE2E ' + randomNumber
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
      name: 'Test Events SDK ' + randomNumber,
      description: 'Test Events SDK ' + randomNumber,
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
      name: 'Test Events SDK ' + randomNumber,
      description: 'Test Events SDK ' + randomNumber,
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
    id: 'zabctest-' + randomNumber,
    source: 'urn:uuid:' + providerId,
    time: ZonedDateTime.now(ZoneOffset.UTC).toString(),
    type: eventCode,
    data: {
      test: 'eventsSDKe2e_' + randomNumber
    }
  })
  console.log(publish)
}
```



Sample code to execute
```javascript
const run = async () => {
  let sdkClient = await initSDK()
  let provider = await createProvider(sdkClient)
  eventMetadata = await createEventMetadata(sdkClient, provider.id)
  journalReg = await registerJournallingEndpoint(sdkClient, provider.id, eventMetadata.event_code)
  await sleep(60000)
  journalling = await fetchJournallingPosition(sdkClient, journalReg.events_url)
  await publishEvent(sdkClient, provider.id, eventMetadata.event_code)
};
```

Next lesson: [Ensure events are published](lesson4.md)