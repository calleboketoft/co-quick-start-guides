# Loopback Server Authorization

*Use the in-memory db connector to begin with since this is not a setup guide for multiple servers. The ExtUser is the Extended User model with custom remoteMethods.*

**Prepare server and TypeScript compilation**
Use existing Loopback server or create a new server `slc loopback`
Add `tsconfig.json` and `.settings/tasks.json` to the package
`npm install debug --save`

**Create user model and custom ‘myAuthorizations’ endpoint**
- `npm install --save`
 - `git+https://github.com/calleboketoft/co-loopback-my-authorizations`
 - `git+https://github.com/calleboketoft/co-loopback-context`
- create the ExtUser `slc loopback:model`
 - name: ExtUser
 - data-source: db (memory)
 - base class: User
 - Expose via rest: y
 - plural: ExtUsers
- Add to `common/models/ext-user.js`:
 - `var app = require('../../server/server')`
 - `require('co-loopback-my-authorizations')(app, ExtUser)`
- Add to `server.js`:
 - `require('co-loopback-context')(app, ‘ExtUser’)`

**Enable public APIs and initialise ACLs with some sane defaults**
- Edit ‘model-config.json’:
 - make the APIs for Roles, RoleMappings, and ACLs public
- Add `init-acl-common.ts` to the boot scripts. Will add ACLs, admin role and a pre-configured admin user (calleboketoft@gmail.com / password)

**Start the server and authorization client**
- Compile TypeScript in server (if there is any)
- Start the server
- Start up client-authorization-management app
- Open the client in a browser and set the port to the server

**Angular SDK**
*Generating ngResource models for all endpoints:*
- `lb-ng server/server.js lb-service.js`

**Using authorization without authentication**
- `npm install --save git+https://github.com/calleboketoft/co-loopback-haxxess-token`
- Add bootscript `haxxessTokenInit.ts`: `module.exports = (server) => require('co-loopback-haxxess-token')(server)`

*Note: the authorization management client is built for this EXACT setup, so make sure the user is called ExtUser and none of the other models are tampered with.*
