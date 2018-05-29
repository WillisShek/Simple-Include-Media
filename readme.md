# Simple Include Media
A simple, all-rounded and easy-to-use media queries library in Sass(Scss).

## How to use
```
.example-class {
	@include media(">=phone & <tablet") {
		// your css
	}
}
```
will become
```
@media (min-width: 480px) and (max-width: 767px) {
	.example-class {
		// your css
	}
}
```
**Do not use any space right after the operators `">"`, `"<"`, `">="`, `"<="` and `"="`.**

## What's new in 1.1.x?
It now supports recursive parsing! You can now use the new function `mediaAddExpressions` to add expressions like `"medium": ">phone & <=tablet"` to further manager you media queries. You can find an example under folder "example".
*****Please be careful when adding expression to avoid any endless loop. eg. `"not": "not all and` is not allowed.**

## Getting Started
### Install
#### NPM
In cmd, run `npm i simple-include-media --save-dev` and include `_simple-include-media.scss` inside `dist` in your build. The default directory should be `"node_modules/simple-include-media/dist"`.

#### Manual
Download the file `dist/_simple-include-media.scss` and import it.


### Import
#### Import directly via Scss
Type `@import "path/to/simple-include-media";` in you Scss file.

#### Import via Sass loader of Webpack
In your config file, add `includePaths: ["./path/to/simple-include-media/dist"]` in option of sass-loader and type `@import "simple-include-media";` in you Scss file.

### Use
`@include media($yourExpression) {...}`

### Documentation
[https://willisshek.github.io/Simple-Include-Media/index.html](https://willisshek.github.io/Simple-Include-Media/index.html)


## How does it work?
Every time you `@include media($string)`, it separates the input string according to space. Than it will parse the separated expressions by the following.
1. Check whether it exists in $mediaExpressions. If it does, converts it. It can use different key to represent the same value. So you can use both `"&"` and `"&&"` to represent `"and"`.npm npm
2. It will check whether it contains the operators `">"`, `"<"`, `">="`, `"<="` or `"="`. The format should be `$dimension>=$value`
	- If true, `$value` will be converted into number. If it can find a key in `$mediaBreakpoints`, the corresponding value will be used.
	- If `$dimension` is empty, "width" will be used. If not, it will return the same string. You can use `"w"` for `"width"`, `"h"` for `"height"` and `"a"` for `"aspect-ratio"`.
	- The prefix `"max-"` and `"min-"` are added automatically according to the operator.
	- No space is allow after `$dimension` and operators.
3. If it doesn't exist in $mediaExpressions and doesn't have any operators, it will return the same expression.
	- eg. `"not"` will return `"not"`.
	- So it uses the same logic as css media does.


## A more complicated example
```
.example-class {
	@include media("! <desktop and landscape") {
		// first css
	}
	@include media("h>360px | a>=1") {
		// second css
	}
}
```

will be compiled into

```
@media not all and (max-width: 1023px) and (orientation: landscape) {
	.example-class {
		// first css
	}
}
@media (min-height: 361px), (min-aspect-ratio: 1) {
	.example-class {
		// second css
	}
}
```


## Important variables
*****No space is allow in the key of variables.**

### $mediaBreakpoints
- Control the value when using `">"`, `"<"`, `">="`, `"<="` or `"="`.
- Do not use `">"`, `"<"`, `">="`, `"<="` or `"="` as key.

### $mediaExpressions
- Short form used to fast conversion of expressions.
- It is not recommend but you can still use operators `">"`, `"<"`, `">="`, `"<="` or `"="` in the key.
- You can use operators as value.
- eg. `">=t": ">=tablet"`. In such case, `">=t"` will become `">=tablet"` and become `"(min-width: 768px)"` finally.

### $mediaUnitSteps
- Control the value added/subtracted when using `">" `or `"<"`.
- Change it if you want to be more precise or rough, or use unit other than px, em and rem.

## Important functions

### mediaAddBreakpoints
- Add new breakpoint(s) without erasing the old one.
- eg. `$mediaBreakpoints: mediaAddBreakpoints(("retina": 1920px)) // be carefull there are double brackets`;
- Due to the limitation of scss, you must add `$mediaBreakpoints:` at the beginning;
- It's the same as `$mediaBreakpoints: map-merge($mediaBreakpoints, ("retina": 1920px));`, but more convenience.

### mediaAddExpressions
- Add new expression(s) without erasing the old one.
- eg. `$mediaExpressions: mediaAddExpressions(("small": "<=phone")) // be carefull there are double brackets`;
- Due to the limitation of scss, you must add `$mediaExpressions:` at the beginning;
- It's the same as `$mediaExpressions: map-merge($mediaExpressions, ("small": "<=phone"));`, but more convenience.

## Remarks

Remember to add `;` to the end of every `@import` to avoid potential error.
