## Eslint TypeScript

Install dependencies

`npm install --save-dev @typescript-eslint/eslint-plugin @typescript-eslint/parse eslint-plugin-react`

Add lint script to `package.json`

`"lint": "eslint . --ext .js,.jsx,.ts,.tsx"`

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