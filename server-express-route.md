# Express with Route

```javascript
var port = 3000
var express = require('express')
var app = express()
var myRouter = express.Router()
myRouter.get('/myroute', (req, res) => {
  res.send({'any':'object'})
})
var server = app.listen(port, () => {
  console.log('listening at port: ' + port)
})
app.use(myRouter)
```