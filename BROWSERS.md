
## Browser compatibility

This is a list of issues arising from cross-browser development we've learned throught the years. When developing, keep in mind the compatibility your application need to have (hopefully IE is out of the mix, but doublecheck it). The perfect philosophy is to avoid anything that could give problems, especially if it can be done in other ways.

### JavaScript

#### [`Element.getBoundingClientRect()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/getBoundingClientRect)

Usually it returns a `DOMRect` object which is the union of the rectangles returned by `getClientRects()` for the element, i.e., the CSS border-boxes associated with the element.
The result is the smallest rectangle which contains the entire element, with read-only `left`, `top`, `right`, `bottom`, `x`, `y`, `width`, and `height` properties describing the overall border-box in pixels. Properties other than `width` and `height` are relative to the top-left of the viewport.

##### Issues

- **IE 9, 10, 11**
  - The returned object lacks `x` & `y` values, use `left` instead of `x` and `top` instead of `y`.
- **IE 8**
  - The returned object lacks `width` and `height` properties.
  - The returned object lacks `x` & `y` values, use `left` instead of `x` and `top` instead of `y`.
- **IE 6, 7**
  - The returned object lacks `width` and `height` properties.

### SVG

#### [`alignment-baseline`](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/alignment-baseline)

The `alignment-baseline` attribute specifies how an object is aligned with respect to its parent. This property specifies which baseline of this element is to be aligned with the corresponding baseline of the parent.

| Chrome                                            | Firefox                                             | Safari                                            | IE11                                      |
| ------------------------------------------------- | --------------------------------------------------- | ------------------------------------------------- | ----------------------------------------- |
| ![chrome](./images/alignment-baseline-chrome.png) | ![firefox](./images/alignment-baseline-firefox.png) | ![safari](./images/alignment-baseline-safari.png) | ![ie](./images/alignment-baseline-ie.png) |

Then it's better to use  `dominant-baseline`.

#### [`dominant-baseline`](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/dominant-baseline)

The `alignment-baseline` attribute specifies the dominant baseline, which is the baseline used to align the box’s text and inline-level contents. It also indicates the default alignment baseline of any boxes participating in baseline alignment in the box’s alignment context.

| Chrome                                           | Firefox                                            | Safari                                           | IE11                                     |
| ------------------------------------------------ | -------------------------------------------------- | ------------------------------------------------ | ---------------------------------------- |
| ![chrome](./images/dominant-baseline-chrome.png) | ![firefox](./images/dominant-baseline-firefox.png) | ![safari](./images/dominant-baseline-safari.png) | ![ie](./images/dominant-baseline-ie.png) |

If you need to support also IE, then use `dy` instead of `dominant-baseline`.

## `<foregnObject>`

This SVG element should enable writing an HTML snippet inside an SVG. It gives all sorts of problems, so it's better NOT to use it at all. A similar result is obtainable with an absolute div over the SVG, where HTML elements can be absolutely positioned with the same or similar coordinates.

Following is a list of problems you'll find if absolutely have to use it:

- In Safari, inside of a `foregnObject` the elements with an opacitywill be moved at `x=0`.
    SOLUTION: use a computed color to obtain the same effect without opacity.

- In Safari, inside of a `foreignObject` any element with `position: absolute` gives problems, using as origin not the `foreignObject` one but the one of the SVG element.
    SOLUTION: insert in the `foreignObject` a div with `position: fixed`, align the `foreignObject` to `y=0` and move it vertically with a `transform="translate()"`.

- In Safari, a `foreignObject` within a `<g>` with a `transform="translate()"`, the `x` translation is not applied.
    SOLUTION: insert in the `foreignObject` a `div` with `position: fixed`.

## `transform`

Small incoherency that browsers treat differently (Firefox and Chrome are more tolerant, Safari is stricter and errors out):
- SVG: `<g transform="translate(10, 10)">` (`transform` is an attribute; no unit of measure)
- HTML: `<div style="transform: translate(10px, 10px)">` (`transform` is a style; unit of measure is mandatory!)
- SVG with d3: `.attr('transform', 'translate(10, 10)')`
- HTML with d3: `.style('transform', 'translate(10px, 10px)')`

NB: an `<svg>` is an HTML element! (and it can also be used as an SVG element, but it's a different story)
