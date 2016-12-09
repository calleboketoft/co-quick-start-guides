# Bootstrap SCSS build

- Install Bootstrap:

>npm install --save-dev bootstrap@4.0.0-alpha.5

- Install node-sass

>npm install --save-dev --save-exact node-sass

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

- If you want Bootstrap 2 buttons for Bootstrap 4:

>npm install --save-dev @calle/bootstrap-2-buttons