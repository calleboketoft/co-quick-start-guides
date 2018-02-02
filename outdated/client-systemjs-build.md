# Client SystemJS build

- create `systemjs-build-task.js`:

```javascript
var Builder = require('systemjs-builder')
var paths = require('./client-paths')

// sets the baseURL and loads the configuration file
var builder = new Builder('.', paths.src + '/systemjs.config.js')

console.log('SystemJS builder starting...')
builder
  .buildStatic(paths.src + '/app/bootstrap.js', paths.build + '/build.js', {
    minify: true,
    mangle: false,
    sourceMaps: true
  })
  .then(() => {
    console.log('Build complete')
  })
  .catch((err) => {
    console.log('Build error')
    console.log(err)
  })
```

- run the script `node systemjs-build-task.js`