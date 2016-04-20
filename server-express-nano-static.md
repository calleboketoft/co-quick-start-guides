# Server - Express Nano Static
*Minimal static server*

```javascript
var port = 3000
var staticDir = './'
var express = require('express')
var app = express()
app.use(express.static(staticDir))
var server = app.listen(port, () => {
  console.log('serving at: ' + port)
})
```
```javascript

- add to package.json:

```json
"scripts": {
  "start": "node server.js"
}
```