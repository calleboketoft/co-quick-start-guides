Minimal project with TypeScript

- >mkdir proj
- >cd proj && npm init -y
- >npm install --save-dev typescript http-server
- >mkdir public
- Add to `package.json`:

```json
{
  "scripts": {
    "build": "tsc public/*.ts",
    "watch": "tsc public/*.ts --watch",
    "serve": "http-server"
  }
}
```

- Add file `public/index.ts`:

```typescript
console.log('test');
```

- Add file `public/index.html`:

```html
<script src="index.js"></script>
```

- Open terminal and run `npm run watch`
- Open another terminal and run `npm run serve`
- Open browser and navigate to `http://localhost:3000`