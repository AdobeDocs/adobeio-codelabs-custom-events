## Lesson 2: Register an event provider

### Prerequisite 
Export 
EVENTS_BASE_URL='https://api.adobe.io'
EVENTS_INGRESS_URL='https://eventsingress.adobe.io'

### Create a project 
In this project, one usually have  
* package.json
* helloworld.js
* .env (optional)

1. package.json
```javascript
{
  "name": "helloworld",
  "version": "1.0.0",
  "repository": "https://github.com/adobe/aio-lib-events/",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "dotenv": "^8.1.0",
    "@js-joda/core": "^2.0.0",
    "@adobe/aio-lib-events": "1.0.0"
  }
}
```
2..env 
Filling in the information from lesson 1 console integration
```javascript
EVENTS_ORG_ID=
EVENTS_API_KEY=
EVENTS_JWT_TOKEN=
EVENTS_WORKSPACE_ID=
EVENTS_PROJECT_ID=
EVENTS_INTEGRATION_ID=
```
3. helloworld.js
```javascript
const EventsSDK = require('@adobe/aio-lib-events')
const path = require('path')
const ZonedDateTime = require('@js-joda/core').ZonedDateTime
const ZoneOffset = require('@js-joda/core').ZoneOffset
// load .env values in the e2e folder, if any
require('dotenv').config({ path: path.join(__dirname, '.env') })

const orgId = process.env.EVENTS_ORG_ID
const apiKey = process.env.EVENTS_API_KEY
const accessToken = process.env.EVENTS_JWT_TOKEN
const consumerOrgId = process.env.EVENTS_CONSUMER_ORG_ID
const workspaceId = process.env.EVENTS_WORKSPACE_ID
const projectId = process.env.EVENTS_PROJECT_ID
const integrationId = process.env.EVENTS_INTEGRATION_ID
```

Next lesson: [Well done](welldone.md)