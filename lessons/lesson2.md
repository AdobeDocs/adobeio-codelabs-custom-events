## Lesson 2: Start a project 

### Create a project 
In this project, one usually have  
* package.json
* usage
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
2. Fill in the required credentials 
Filling in the information from lesson 1 console integration
```javascript
EVENTS_ORG_ID=
EVENTS_API_KEY=
EVENTS_JWT_TOKEN=
EVENTS_WORKSPACE_ID=
EVENTS_PROJECT_ID=
EVENTS_INTEGRATION_ID=
```
3. Usage
Initialize the SDK
```javascript
const sdk = require('@adobe/aio-lib-events')

async function sdkTest() {
  //initialize sdk
  const client = await sdk.init('<organization id>', 'x-api-key', '<valid auth token>', '<options>')
}

```
Call methods using the initialized SDK
```javascript
const sdk = require('@adobe/aio-lib-events')

async function sdkTest() {
  // initialize sdk
  const client = await sdk.init('<organization id>', 'x-api-key', '<valid auth token>', '<options>')

  // call methods
  try {
    // get profiles by custom filters
    const result = await client.getSomething({})
    console.log(result)

  } catch (e) {
    console.error(e)
  }
}

```

Next lesson: [Explore custom event SDK](lesson3.md)