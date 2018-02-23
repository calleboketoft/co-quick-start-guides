# Prettier.js

Install dependencies

`npm install --save-dev prettier tslint-config-prettier`

Add base `tslint.json` file

```json
{
  "extends": [
    "./existing-tslint-file.json",
    "tslint-config-prettier"
  ]
}
```

Add file `.prettierrc`

```json
{
    "printWidth": 80,
    "singleQuote": true,
    "useTabs": false,
    "tabWidth": 4,
    "semi": false,
    "bracketSpacing": true
}
```

Add npm script `format`

```bash
"format": "prettier --write \"src/**/*.ts\""
```