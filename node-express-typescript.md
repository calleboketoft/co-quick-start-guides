##Get started with Node, TypeScript, and Express

First of all, Node needs to be installed on the computer.

- `>npm install -g typescript@next tsd` Install TypeScript and TSD if they're not already present. `next` is needed for the exclude array in `tsconfig.json`

***

- `>mkdir myNewProj && cd myNewProj`

- `>npm init`

- `>tsd install node`

**Create the following files**

`.gitignore`:

```bash
node_modules
```

`tsconfig.json`:

```json
{
  "compilerOptions": {
    "target": "ES5",
    "module": "commonjs",
    "sourceMap": true,
    "watch": true
  },
  "exclude": [
    "node_modules"
  ]
}
```

`server/server.ts`:

```javascript
/// <reference path="../typings/node/node.d.ts" />
console.log('server started')
```

###Alt 1: Terminal development

- `>cd server && tsc -w` Start the TypeScript compilation watcher in one terminal

- `>node server/server` Start the Node server in a second terminal

###Alt 2: VSC (Visual Studio Code) development

*Note that VSC is in beta, so some stuff isn't working 100% properly yet*

VSC needs a couple of configuration files to get "tasks" and Node debugging up and running:

`.settings/launch.json`:

```json
{
  "version": "0.0.1",
  "configurations": [
    {
      "name": "my server",
      "type": "node",
      "program": "server/server.js",
      "stopOnEntry": false,
      "cwd": ".",
      "sourceMaps": true
    }
  ]
}
```

`.settings/tasks.json`

```json
{
  "version": "0.1.0",
  "command": "tsc",
  "isShellCommand": true,
  "showOutput": "silent",
  "problemMatcher": "$tsc"
}
```

- Open up the project `myNewProj` in VSC

- Start TypeScript compilation watcher in VSC `cmd + shift + b`

- Start the server by pressing `F5` in VSC or going to the debug page and pressing the start button.

###Adding Express

`>npm install express --save`

`>server/server.ts`:

```javascript
/// <reference path="../typings/node/node.d.ts" />
let express = require('express')
let app = express()

app.get('/', (req, res) => {
  res.send('Hello World!')
})

let server = app.listen(3000, () => {
  let host = server.address().address
  let port = server.address().port

  console.log('Example app listening at http://%s:%s', host, port)
})
```

###Adding Loopback

remove the folder `server`

`>npm install -g strongloop`

`>slc loopback` in the `myNewProject` dir

Then just change to `.ts` for the files that are going to be TypeScript