# dart-sass-issue-3322

https://github.com/sass/sass/issues/3322

## Replicate
1. `npm i`
2. `sass _index.scss test.css`

## Expected
**test.css**
```css
.test :root {
  --some-var: 123px;
}
.test .animate--fade-in, .test .modal__backdrop {
  color: red;
}
.test .modal__backdrop {
  animation-duration: 0.2s;
}

/*# sourceMappingURL=test.css.map */

```

or a more accurate error detail than `Error: The target selector was not found.` if we are not using the Module system as expected.

## Actual
```
Error: The target selector was not found.
Use "@extend .animate--fade-in !optional" to avoid this error.
  ╷
9 │     @extend .animate--fade-in;
  │     ^^^^^^^^^^^^^^^^^^^^^^^^^
  ╵
  _mixins.scss 9:5  root stylesheet
```

## Observations

* Sass doesn't throw an error if you do one of the following:
  * Commenting out `@use 'mixins';` in `_index.scss`
    * Outputs as expected
  * Removing the `:root {}` selector from `_variables-css.scss`
    * Outputs without error (and without `:root {}`, obviously)
    * Moving `:root {}` anywhere else in the project outputs as expected
  * Commenting out `@include meta.load-css('modal');`
    * Outputs without error (no modal styles, obviously)
