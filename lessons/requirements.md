## Requirements

### Developer Console Access
A pre-requisite of the codelab is to have access to [Developer console](https://console.adobe.io/home) 

### Pre-requisites

- [NodeJS](https://nodejs.org/en/download/) (at least v12). It should also install npm together. We recommend using [nvm](https://github.com/nvm-sh/nvm/blob/master/README.md) to manage NodeJS installation and versions on your machine.  
- [Visual Studio Code (VSCode)](https://code.visualstudio.com/download) as the supported IDE for editor, debuggger, etc. You can use any other IDE as a code editor, but advanced usage e.g. debugger is not yet supported.

### Command Line Interface

Install the [Adobe I/O Events Lib](https://github.com/adobe/aio-lib-events)
```bash
npm install @adobe/aio-lib-events
```  
### Console Integration

We assume you have acess to create integration on I/O Console with I/O Management Service needs to be enabled for the integration. This will help create the JWT token with adobeio_api scope which is required for all the API calls. 

Start the first lesson: [Create a Console Integration](/lessons/lesson1.md).