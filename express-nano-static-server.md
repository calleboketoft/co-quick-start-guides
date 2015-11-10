# Express Nano Static Server
*Minimal static server*

```javascript
var port = 3005
var staticDir = './www'
var express = require('express')
var app = express()
app.use(express.static(staticDir))
var server = app.listen(port, function () {
  console.log('Listening at ' + port)
})
```
