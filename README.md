# Next.js Integration with Jscrambler

This integration example is based on a fork of the `learn-starter` template, which can be found [here](https://github.com/vercel/next-learn-starter/tree/master/learn-starter).

Start by cloning `nextjs-jscrambler-integration-tutorial`:

```
git clone https://github.com/JscramblerBlog/nextjs-jscrambler-integration-tutorial.git
```

Install the required dependencies:

```
cd nextjs-jscrambler-integration-tutorial
npm i
```

Run the cloned app:

```
npm run dev
```

Check if everything looks right by opening the app on the local development server.

## Configure Jscrambler
If you haven't tried Jscrambler out before reading this tutorial, please consider reading the [Getting Started Guide](https://docs.jscrambler.com/code-integrity/getting-started), which will walk you through the steps on how to protect your application. This will make this section easier to grasp. It will also teach you how to configure Jscrambler and use a custom configuration.

To complete the integration with Jscrambler, you need a JSON configuration file with your API credentials, application ID, and protection configuration. You may create your transformations recipe using the [Jscrambler Web application](https://app.jscrambler.com/) and download a JSON configuration file or use the following example for a quick test. This file should be named `jscrambler.json` and placed on the project's root folder. If you choose to try the following example, just make sure to fill the missing information: `accessKey`, `secretKey` and `applicationId`.

Create a `.jscramblerrc` file on the project's root folder.

Select the desired configuration. To do it quickly, visit the [Jscrambler Web App](https://app.jscrambler.com/dashboard) and create a new app. Select the transformations you want (check the Templates and Fine-Tuning tabs). In this tutorial, we used the Obfuscation template. For further help, see our [guide](https://blog.jscrambler.com/jscrambler-101-how-to-use-the-cli/).

Download the **JSON config file**.

![Download Jscrambler JSON](https://blog.jscrambler.com/content/images/2021/02/jscrambler-app-download-JSON-4.gif)

Open `jscrambler.json` and copy all its contents to .jscramblerrc. Add two new sections to `.jscramblerrc`: `filesSrc` and `filesDest` (see below). Your final `.jscramblerrc` file should look like this:

```json
{
 "keys": {
   "accessKey": <ACCESS_KEY_HERE>,
   "secretKey": <SECRET_KEY_HERE>
 },
 "applicationId": <APP_ID_HERE>,
 "filesSrc": [
   "./.next/**/*.html",
   "./.next/**/*.js"
 ],
 "filesDest": "./",
"params": [
    {
      "name": "objectPropertiesSparsing"
    },
    {
      "name": "variableMasking"
    },
    {
      "name": "whitespaceRemoval"
    },
    {
      "name": "identifiersRenaming",
      "options": {
        "mode": "SAFEST"
      }
    },
    {
      "name": "globalVariableIndirection"
    },
    {
      "name": "dotToBracketNotation"
    },
    {
      "name": "stringConcealing"
    },
    {
      "name": "functionReordering"
    },
    {
      "options": {
        "freq": 1,
        "features": [
          "opaqueFunctions"
        ]
      },
      "name": "functionOutlining"
    },
    {
      "name": "propertyKeysObfuscation",
      "options": {
        "encoding": [
          "hexadecimal"
        ]
      }
    },
    {
      "name": "regexObfuscation"
    },
    {
      "name": "booleanToAnything"
    }
  ],
  "areSubscribersOrdered": false,
  "useRecommendedOrder": true,
  "jscramblerVersion": "<7.X>",
  "tolerateMinification": true,
  "profilingDataMode": "off",
  "useAppClassification": true,
  "browsers": {}
}
```

## Integrating Jscrambler in the Build Process
You can integrate Jscrambler into the Next.js build process with the CLI.

```bash
npm i jscrambler --save-dev
```

Create a CLI hook in the *scripts* section of `package.json`. The section should look like this:

```json
  "scripts": {
    "dev": "next dev",
    "build": "next build && jscrambler",
    "start": "next start"
  },
```

The `"build": "next build && jscrambler"` hook will trigger the `jscrambler` command after the build process is finished.

Build the application:

```
npm run build
```

There you go. Jscrambler is now integrated in your build process and the protected production files will be placed on `.next/static/`.

## Testing the Protected App
Run the app:

```
npm run start
```

Open the URL provided in the console and it will open up a server with the production files.

Now, you can check what your protected code looks like by opening the browser's debugger and opening the files from the "Sources" tab.
