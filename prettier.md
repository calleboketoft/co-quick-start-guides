# Prettier.js

Install dependencies

`npm install --save-dev prettier`

Add file `.prettierrc.json`

```json
{
    "printWidth": 80,
    "singleQuote": true,
    "useTabs": false,
    "tabWidth": 2,
    "bracketSpacing": true
}
```

Add npm script `format`

```bash
"format": "prettier --write \"src/**/*.ts\""
```
