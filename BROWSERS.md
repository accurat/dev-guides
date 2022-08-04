
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

## `foregnObject`

- In Safari, nei div dentro a un `foregnObject` sembra che se si applica un'opacità essi vengono spostati a `x=0` (???)

  _soluzione/workaround_: usare colore anzichè opacità

- In Safari, in un `foreignObject` qualunque elemento con `position: absolute` dà problemi, trattando come origine non quella del `foreignObject` ma quella dell'elemento SVG.

  _soluzione/workaround_: inserire nel `foreignObject` un div in position fixed, allineare il `foreignObject` alla `y=0` e muoverlo verticalmente con un translate

- In Safari, in un `foreignObject` dentro a un `<g>` che abbia un `translate`, la `x` non viene applicata.

  _soluzione/workaround_: inserire nel `foreignObject` un `div` in position `fixed`

## `transform`

Piccola incoerenza che i browser trattano diversamente (Firefox e Chrome tolleranti, Safari più strict):
- in SVG si fa: `<g transform="translate(10, 10)">` mentre in HTML: `<div style="transform: translate(10px, 10px)">`
- in SVG si fa: `.attr('transform', 'translate(10, 10)')` mentre in HTML: `.style('transform', 'translate(10px, 10px)')`
NOTA BENE: un `<svg>` è un elemento HTML, non SVG
