## Eslint TypeScript

Install dependencies

`npm install --save-dev @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint-plugin-react`

Add lint script to `package.json`

`"lint": "eslint . --ext .js,.jsx,.ts,.tsx"`

Add TypeScript compiler check to `package.json`

`"check-ts": "tsc --noEmit -p tsconfig.json",`

Add `.eslintrc.js`

```javascript
module.exports = {
  root: true,
  parser: '@typescript-eslint/parser',
  plugins: [
    '@typescript-eslint',
  ],
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:react/recommended'
  ],
  settings: {
    react: {
      version: 'detect'
    }
  }
};
```

If using create-react-app, override eslint like this

Create `.env` file in project root
```
EXTEND_ESLINT=true
```

Remove `node_modules` if your rules file isn't respected