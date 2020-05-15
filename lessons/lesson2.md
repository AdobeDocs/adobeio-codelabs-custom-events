## Lesson 2: Set up Project

In this project, you will do 
* add `package.json` for your project
* Fill in `.env` file with needed information 
* Set environment varialbles   

1. `package.json` sample file
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
Filling in the information from lesson 1 console integration, you can choose import from `.env` file or other ways you are comfortable with 
```javascript
EVENTS_ORG_ID=
EVENTS_API_KEY=
EVENTS_CONSUMER_ORG_ID=
EVENTS_WORKSPACE_ID=
EVENTS_PROJECT_ID=
EVENTS_INTEGRATION_ID=
```

3. Set environment varialbles
run in terminal or use IDE 
```bash
export EVENTS_INGRESS_URL='https://eventsingress-stage.adobe.io'
export EVENTS_BASE_URL='https://api-stage.adobe.io'
```


### Usage 
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