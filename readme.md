# simple-include-media
A simple, all-rounded and easy-to-use media queries library in Sass(Scss).

## How to use?

```
.example-class {
	@include media("! all & <desktop and landscape") {
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

**Do not use any space after the operators `">"`, `"<"`, `">="`, `"<="` and `"="`.**


## How does it work?
Every time you @include media($string), it separates the input string according to space. Than it will parse the separated expressions by the following.
1. Check whether it exists in $mediaExpressions. If it does, converts it. It can use different key to represent the same value. So you can use both `"&"` and `"&&"` to represent `"and"`.
2. It will check whether it contains the operators `">"`, `"<"`, `">="`, `"<="` or `"="`. The format should be `$dimension>=$value`
	- If true, `$value` will be converted into number. If it is alphabetic string, $mediaBreakpoints will be used.
	- If `$dimension` is empty, "width" will be used. If not, it will return the same string. You can use `"w"` for `"width"`, `"h"` for `"height"` and `"a"` for `"aspect-ratio"`.
	- The prefix `"max-"` and `"min-"` are added automatically according to the operator.
	- No space is allow after `$dimension` and operators.
3. If it doesn't exist in $mediaExpressions and doesn't have any operators, it will return the same expression.
	- eg. `"not"` will return `"not"`.
	- So it uses the same logic as css media does.

You can check the above example for a complicated use.


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

## Remarks

You can use `map-merge` too add keys without erasing the default values.
- eg. `$mediaBreakpoints: map-merge($mediaBreakpoints, ("XL": 1900px));`

For node-sass users, remember to add ; to the end of every @import.
