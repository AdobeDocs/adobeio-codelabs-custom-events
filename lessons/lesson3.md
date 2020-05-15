## Lesson 3: Explore Custom Event SDK

### Export the right url
Export 
EVENTS_BASE_URL='https://api.adobe.io'
EVENTS_INGRESS_URL='https://eventsingress.adobe.io'

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
Filling in the information from lesson 1 console integration
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

### Create journal registration 
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
### Use webhook 

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

Next lesson: [Ensure events are published](lesson4.md)