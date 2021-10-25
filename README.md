# NewRelic-Typescript Integration

This example demonstrates how a regular Typescript project can be instrumented with New Relic.

The non-instrumented application can be easily inspected if you checkout the 'regular-typescript' tag:

```sh
git checkout tags/regular-typescript -b regular-typescript
```

To go back to the instrumented project, execute: ```git checkout master -f```

## Compatibility considerations

Typescript is a **strongly typed** language. Because of that, any external package (like NewRelic) needs to be complemented with a [_declarations_][https://www.typescriptlang.org/docs/handbook/2/type-declarations.html] file.

The _declarations_ file can be either installed from a **@types** package or created manually.

To keep this demo simple, we are going to use the _declarations_ from the [package][https://www.npmjs.com/package/@types/newrelic] **@types/newrelic**.

### IMPORTANT NOTE!
As of the date this demo was programmed, Oct 2021, the package **@types/newrelic** was not up-to-date with the latest version of the agent v8.5.0. However the package works perfectly well for version v7.5.2 of the agent. This demo uses the latter.

If you want to use the latest version of the agent, you will need to create and include the _declarations_ by yourself. For an example on how to achieve that, check out the [Stackoverflow answer][https://stackoverflow.com/questions/43596582/declararion-file-for-3th-party-module-in-typescript].

## Instructions to instrument our regular Typescript project with NewRelic

1. Install NewRelic agent v7.5.2:
```
npm install --save newrelic@7.5.2
```

2. Install NewRelic Types definitions in development packages:
```
npm install --save-dev @types/newrelic
```

3. Copy _newrelic.js_ file to any place in your code. We moved it to source directory:
```
cp node_modules/newrelic/newrelic.js src/
```

4. Edit _src/newrelic.js_ file to include your LICENSE KEY, APPLICATION_NAME and other configurations you may need. 

5. **This step is very important for the agent to work**. Modify Build Phase on **package.json** to copy _src/newrelic.js_ to the distribution folder. 
In our case we changed this:
```
  "build": "tsc",
```
into this:
```
  "build": "tsc && cp src/newrelic.js dist/newrelic.js ",

```

6. Modify the main script _src/index.ts_ to import newrelic **_on the very top_**:
```
import newrelic from "newrelic";

```

7. Invoke a harmless, valid method from _newrelic_ to force Typescript compiler to include it in the final Javascript main file (_dist/index.js_).
In our case we just added this, before creating the express app:
```
const transaction = newrelic.getTransaction();

```

8. Finally, stop and restart your server:
```
npm run start
```

## Base Typescript project borrowed from here

https://developer.okta.com/blog/2018/11/15/node-express-typescript
