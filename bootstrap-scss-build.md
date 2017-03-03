# Bootstrap SCSS build

- Install Bootstrap:

>yarn add --dev --exact bootstrap@4.0.0-alpha.6

- Install node-sass

>yarn add --dev --exact node-sass

- Add SASS script to package.json

```json
"sass": "node-sass ./scss/main.scss ./client/assets/css/main.css"
```

- Add folder `scss` and file `main.scss` with content like this:

```scss
@import 'bootstrap-overrides';
@import '../node_modules/bootstrap/scss/bootstrap';
@import '../node_modules/@calle/bootstrap-2-buttons/bootstrap-2-buttons';
@import 'custom-styles';
```

- Add bootstrap overrides to the file `scss/bootstrap-overrides.scss` like this:

```scss
// Bootstrap overrides
//
// Copy variables from `_variables.scss` to this file to override default values
// without modifying source files.
// =============================================================================

// Typography
//
// Font, line-height, and color for body text, headings, and more.

// Pixel value used to responsively scale all typography. Applied to the `<html>` element.
$font-size-root: 14px !default;
```

- Compile SCSS by running `npm run sass`

- Add css to header of `index.html`:

```html
<link href="assets/css/main.css" rel="stylesheet" />
```

- If you want Bootstrap 2 buttons for Bootstrap 4:

>yarn add --dev --exact @calle/bootstrap-2-buttons
