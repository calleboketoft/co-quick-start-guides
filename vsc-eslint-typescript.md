# Visual Studio Code ESlint TypeScript

## Enable ESLint for VSC
Add this to your VSC settings:
```json
  "eslint.enable": true,
  "eslint.validate": [
    "typescript"
  ],
```

## Enable TypeScript in ESLint

Install needed plugins in project like this
>yarn add --exact --dev eslint eslint-plugin-typescript typescript-eslint-parser

Add `.eslintrc.json` file:
```json
{
    "plugins": [
        "typescript"
    ],
    "parser": "typescript-eslint-parser",
    "rules": {
        // due to TypeScript
        "no-undef": "off",
        "no-unused-vars": "off",
        "no-useless-constructor": "off",

        // exceptions
        "padded-blocks": "off",

        // TypeScript specific
        "typescript/type-annotation-spacing": "error",
        "typescript/explicit-member-accessibility": "error"
    },
    "parserOptions": {
        "sourceType": "module"
    }
}
```

## Adding `standard` to the setup (not sure if relevant)

Install standard plugins
>yarn add --exact --dev eslint-plugin-standard eslint-plugin-promise eslint-config-standard

Update `.eslintrc.json` file:
```json
{
    "extends": "standard",
    "plugins": [
        "standard",
        "promise",
        "typescript"
    ],
    "parser": "typescript-eslint-parser",
    "rules": {
        // due to TypeScript
        "no-undef": "off",
        "no-unused-vars": "off",
        "no-useless-constructor": "off",

        // exceptions
        "padded-blocks": "off",

        // TypeScript specific
        "typescript/type-annotation-spacing": "error",
        "typescript/explicit-member-accessibility": "error"
    },
    "parserOptions": {
        "sourceType": "module"
    }
}
```

## Related links

ESlint plugin for VSC
- https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint

TypeScript parser for ESLint, needed for ESLint to understand TypeScript
- https://github.com/eslint/typescript-eslint-parser

TypeScript plugin with specific TS rules for ESLint
- https://github.com/nzakas/eslint-plugin-typescript
