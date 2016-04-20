# Server - Loopback Static

- `slc loopback`
- remove all files that look redundant from the generated package
- remove `server/boot/root.js`
- add to `server/middleware.json`:

```json
"files": {
  "loopback#static": {
    "params": "$!../client"
  }
}
```

- add to package.json:

```json
"scripts": {
  "start": "node server/server.js"
}
```