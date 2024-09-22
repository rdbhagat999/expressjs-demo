# Express JS Demo Source Code

## Features

- Examples of `GET`, `POST`, `PUT`, `PATCH`, and `DELETE` requests.
- Modular routers & organized endpoints. See `src/routes`.
- Request Validation using [`express-validator`](https://express-validator.github.io/docs).
- Examples of custom middleware implementation
- Cookies, Sessions & Session Store Implementation
- Connecting to MongoDB using [Mongoose](https://mongoosejs.com/docs/)
- Authentication using [Passport.js](https://www.passportjs.org/) featuring local strategy and OAuth2 using Discord (can be replaced with your own provider.)
- Hashing Passwords using [bcrypt](https://www.npmjs.com/package/bcrypt).
- Unit Testing examples using [Jest](https://jestjs.io/). See `src/__tests__`.
- Integration & E2E Testing examples using [Supertest](https://www.npmjs.com/package/supertest). See `src/e2e`.

## Installation

_Note_: You will need MongoDB running locally on your machine.

1. Clone this repository
2. Run `npm install`
3. Run `npm run start:dev`
4. Server will connect to MongoDB & run on Port 3000.

## FAQ

**Q: Why are the files `.mjs` and not `.js`?**

`.mjs` is the file extension for Javascript code written in an ESM Module. You know how Node.js by default doesn't allow you to use `import` and `export` statements. That's because the default module system Node uses is `CommonJS`. In order to use those modern syntactic keywords, you must set your project `type` to be `module`. See the `package.json`.

**Q: What is createApp.mjs?**

I created this file to setup integration tests. This was required because I needed to be able to export the Express app instance into the test files, without importing other code that performs side effects, such as the connection to the database or the `app.listen` call, both inside the `src/index.mjs` file.

## How can I use another OAuth2 Provider? Such as Google, Twitter, Facebook, etc

1. Create a separate file inside the `src/strategies` folder, call it `[provider-name]-strategy.mjs`, _e.g: google-strategy.mjs_

2. Go to the provider's platform's developer console, and create a Developer Application. You want to make sure the Application is an OAuth Client or provides an OAuth Client.

3. When configuring the OAuth Client, be sure to set a [Redirect URI](https://www.oauth.com/oauth2-servers/redirect-uris/). This is for the platform to redirect back to your web server.

4. Set any scopes that your application needs. This allows your application to retrieve the necessary private information of the user authenticating in to your app!

5. Once the OAuth Client application is created, be sure to copy the Client ID and Client Secret.

   _Note: The Client Secret MUST be kept private on the server!_

6. Install the correct passport strategy. Go to [passportjs.org/packages](https://www.passportjs.org/packages/) and search for the strategy for the OAuth2 Provider you are using.

   e.g: If you are using Google, then you'd want [`passport-google`](https://www.passportjs.org/packages/passport-google-oauth20/)

7. Use the `strategies/discord-strategy.mjs` file as a starter template. Most of the code in there will be the same, with some modification. You need to import the `Strategy` class from the strategy you installed. If you're using Google, then you would import `Strategy` from `passport-google-oauth20`.

   e.g: `import { Strategy } from 'passport-google-oauth20'`

8. Replace the `clientID`, `clientSecret`, `callbackURL`, and `scope` in the Strategy Options object passed to the Strategy constructor with your own provider's OAuth client values.
