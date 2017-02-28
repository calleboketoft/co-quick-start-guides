# Visual Studio Code ESlint TypeScript

## Enable TypeScript in ESLint

ESlint plugin for VSC
- https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint

TypeScript parser for ESLint, needed for ESLint to understand TypeScript
- https://github.com/eslint/typescript-eslint-parser

TypeScript plugin with specific TS rules for ESLint
- https://github.com/nzakas/eslint-plugin-typescript

Install needed plugins in project like this
>yarn add --exact --dev eslint eslint-plugin-typescript typescript-eslint-parser

Add `.eslintrc.json` file:
```json
{
    "plugins": [
        "typescript"
    ],
    "parser": "typescript-eslint-parser"
}
```

## Adding `standard` to the setup

Install standard plugins
>yarn add --exact --dev eslint-plugin-standard eslint-config-standard

Update `.eslintrc.json` file:
```json
{
    "extends": "standard",
    "plugins": [
        "standard",
        "promise",
        "typescript"
    ],
    "parser": "typescript-eslint-parser"
}
```