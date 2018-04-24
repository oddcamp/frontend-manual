# Kollegorna's Front End Manual
###### v0.4

This document outlines the basic stuff for Front End development at Kollegorna. We should try to keep this as tiny as possible and include only the most important stuff.

These things are the default for all our projects unless anything else is specifically said. The entire Front End team should know these things. If you feel that you need some time of powering up your skills just holla at Per and you'll get it.

## Table of Contents

- [Design](#design)
- [HTML](#html)
  * [Semantics](#semantics)
  * [Templating Languages](#templating-languages)
  * [Resources](#resources)
- [CSS](#css)
  * [Preprocessor](#preprocessor)
  * [Linter](#linter)
  * [File Structure and Boilerplate](#file-structure-and-boilerplate)
  * [Naming](#naming)
  * [Using ID's](#using-ids)
  * [Componentization](#componentization)
  * [Units](#units)
  * [Styled strategy](#styled-strategy)
  * [Responsive Breakpoints](#responsive-breakpoints)
  * [Usability and Accessibility](#usability-and-accessibility)
  * [Performance](#performance)
  * [Fonts](#fonts)
  * [Design Systems](#design-systems)
  * [Vendor Prefixes](#vendor-prefixes)
  * [Resources](#resources-1)
- [JavaScript](#javascript)
  * [Style](#style)
  * [ES6](#es6)
  * [Linter](#linter-1)
  * [jQuery](#jquery)
  * [Animations](#animations)
  * [Routing](#routing)
  * [Resources](#resources-2)
- [Media](#media)
  * [Vector Images (SVG)](#vector-images--svg-)
  * [Raster](#raster)
  * [Icons](#icons)
  * [Screen Sizes and Pixel Density](#screen-sizes-and-pixel-density)
  * [Optimisation](#optimisation)
  * [Resources](#resources-3)
- [Structured Data](#structured-data)
  * [Resources](#resources-4)
- [Libraries](#libraries)
- [Accessibility](#accessibility)
  * [WCAG 2.0 Level AA](#wcag-20-level-aa)
  * [Best Practices](#best-practices)
  * [Resources](#resources-5)
- [Performance](#performance)
  * [Best Practices](#best-practices-1)
  * [Resources](#resources-6)
- [Support and Compatibility](#support-and-compatibility)

## Design

See [Design.md](Design.md)

**[🚡 back to top](#table-of-contents)**

## HTML

### Semantics

We should make an effort to produce valid [semantic HTML](https://codepen.io/mi-lee/post/an-overview-of-html5-semantics), that takes advantage of the full potential of HTML5's tags to produce clean code, that is readable by humans and machines alike.

### Smart Quotes

Use the [correct quotation marks and apostrophes](http://smartquotesforsmartpeople.com) for content:

  * ❌ "Don't be dumb"
  * 👌 “You’re smart!”

### Minimum viable `<head>` tag composition

We recommend using [these tags](https://github.com/kollegorna/frontend-manual/tree/master/examples/head-tags-recomended.html) (as well as [manifest.json](https://github.com/kollegorna/frontend-manual/tree/master/examples/manifest.json)) in HEAD area are of the document as a starting point.

### Learn More

* https://codepen.io/mi-lee/post/an-overview-of-html5-semantics
* http://smartquotesforsmartpeople.com

**[🚡 back to top](#table-of-contents)**

## CSS

We use [Airbnb's css style guide](https://github.com/airbnb/css) as the basis for our CSS methodology and BEM for naming conventions, both with some minor exceptions and adaptations.

### Preprocessor

* We use [SASS](https://sass-lang.com) as the standard for our styling needs.
* We use [Styled Components](https://www.styled-components.com) and [Polished](https://polished.js.org) for JS frameworks based projects.

### Linter

We recommend using [stylelint](https://stylelint.io) with [this config](https://github.com/kollegorna/frontend-manual/tree/master/examples/stylelint-config.json).

### File Structure and Boilerplate

Different projects may require different file structuring, but in general it's a good idea to split styles into several files and, for larger projects, organise them into folders. Please use your best judgement here, and choose the structure that you believe suits the project best. If your CSS files are getting too long, it's probably a good sign that you need to reconsider how files are structured.

We strongly recommend using our [SASS-Boilerplate](https://github.com/kollegorna/sass-boilerplate) in a pair with [SASS-Utils](https://github.com/kollegorna/sass-utils) for SASS projects. Please follow the guidelines provided on the repository pages.

### Naming

* We prefer the use of underscores and dashes over camelCase approach.

* Selectors shouldn't be chained above three levels:
    * 👌 `.settings-nav` is good
    * 👌 `.settings-nav__links` is good
    * ✅ `.settings-nav__links__list` is ok
    * ❌ `.settings-nav__links__list__item` should be avoided

* In case of a need to chain more than three selector levels consider nesting. Nested single class names should start with a dash which indicates that this selector is scoped/local (strictly belongs to the parent selector) and altogether it won't conflict with a probable global component that has the same name:
    * 👌 `.settings-nav .-links` is good
    * ❌ `.settings-nav .links` should be avoided

    Important! Do not confuse the last example with the cases of extending a global component:

    * 👌 `.settings-nav .links` is good if `.links` was a global component and was meant to be extended under the `.settings-nav` component

* For performance reasons, selectors shouldn't nest more than three levels deep:
    * 👌 `.settings-nav .-links__list` is good
    * 👌 `.settings-nav .-links .-list` is good
    * ❌ `.settings-nav .-links .-list .-item` should be avoided

    They also shouldn't nest when there's no reason to:
    * 👌 `.settings-nav ul a` is good
    * ❌ `.settings-nav ul li a` should be avoided

* For secure scoping reasons modifier classes should be formated in BEM manner or concatenated with double-dashed classname:
    * 👌 `.settings-nav__hidden` is good
    * ✅ `.settings-nav.--hidden` is ok
    * ❌ `.settings-nav.hidden` should be avoided

### Using ID's

Even though ID attribute was primarily designed as an accessibility feature for fragmenting document, but the requirement for uniqueness, specificity, componentization are the actual reasons why ID's in CSS should be avoided by any means.

### Componentization

Treating the whole page as a single component can easily get you in a selector chain hell, it becomes difficult to use parent modifier classes that affect child elements, your code becomes spaghetti. Therefore it's recomended to go with React-like approach and treat the page as a combination of multiple and small components, which by the nature are easily reusable, extendable, the code becomes more visually perceivable.

❌ Avoid complex SASS constructions like this:

```scss
.settings {
  // ...

  &__wrapper {
    // ...

    &__nav {
      // ...
    }

    &__sidebar {
      //...

      &__avatar {
        // ...
      }
    }
  }
}
```

👌 Always strive for breaking things into smaller components like this:

```scss
.settings-wrapper {
  // ...
}

.settings-nav {
  // ...
}

.settings-sidebar {
  // ...
}

.settings-avatar {
  // ...
}
```

If you have a component which is reused multiple of times on the same page, it's recommended to avoid deciding on its context (i.e. use positioning related properties such as `margin`, `position/top/left/...`), but put that responsibility on a parental component:

```scss
.settings-avatar {
  // ...
}

.settings-sidebar {
  position: relative;

  .settings-avatar {
    position: absolute;
    top: 0;
    left: 0;
  }
}

.settings-main {
  // ...

  .settings-avatar {
    margin-top: 1.5rem;
  }
}
```

### Units

For a better accessibility we should use EMs/REMs and set the font size of the root element (`html`) to a percentage value, preferably `100%`:

```css
html {
  font-size: 100%;
}
```

This enables the users (most likely visually impaired) to scale the site visually. For example: let's say the root element's font size on our site is as recommend — `100%`. By default the website font size in the browser's settings is set to 16px, which means `1rem = 16px`. But if the user has that option set to `32px` the REM value is going to be affected accordingly: `1rem = 32px`. Accessible!

REMs should be used by default, EMs when we need local dependencies and PXs only for the rare cases when things aren't supposed to scale at all (e.g. 1px thick borders). EMs and REMs should be calculated through a [helper from SASS-Utils](https://github.com/kollegorna/sass-utils#units):

```scss
.component {
  width: em(320); // 320px
  padding: rem(30 20); // 30px 20px
  margin: rem(40 auto 20); // 40px auto
}
```

Direct input of these units should avoided, unless you're in a need for proportial sizing:

```scss
h1 {
  font-size: rem(36); // 36px

  sup {
    font-size: 0.5em; // 18px (half of 36px)
  }
}
```

**Important:** Media Queries have to be EMs based ([where's why](https://zellwk.com/blog/media-query-units)). This is already solved by a helper from [SASS-Utils](https://github.com/kollegorna/sass-utils#mq-mixin).

### "Styled" Strategy

"Styled" is a strategy for styling HTML elements that are usually inserted via WYSIWYG editors when writing articles, blog posts: `h1-6, p, blockquote, a, ul, ol, dl, table, code, pre`, etc.
When starting a new project we prefer every HTML element to be by default unstyled (naked) and unopinionated about the context it's. Site-specific styles should only be added when necessary as the project grows. Benefits are:

* No need to overwrite default styles (e.g. remove margins, change hover effects, etc.) when an element is in different context or should be styled differently;
* No need to track the changes in the default styling of an element and update its every single occurence where the styling was meant to be completely different;
* Visual consistency among browsers;
* Smaller size of the final CSS file.

For further details, usage and tips follow the ["Styled" guide on SASS-boilerplate](https://github.com/kollegorna/sass-boilerplate#styled-strategy) repository page.

### Responsive Breakpoints

You should structure breakpoints to avoid overrides: define shared styles first and put the rest to the appropriate media queries.

```scss
.element {
  color: #fff;
  background-color: #000;
  border-radius: rem(5);
  // ^ shared styles

  @include mq(small down) {
    margin-top: rem(20);
  }

  @include mq(between small large) {
    margin-bottom: rem(20);
  }

  @include mq(large up) {
    position: absolute;
    top: rem(20);
    right: rem(20);
  }
}
```

Benefits are:
* No need to overwrite styles in a media query when changing or adding the default ones.
* Smaller size of the final CSS file.

We use [Media Queries helper](https://github.com/kollegorna/sass-utils#media-queries) from our SASS-Utils library.

### Usability and Accessibility

#### Indicating Interaction

Visually indicating that an element is available to be interacted with (e.g. a button is clickable) or the interaction has been successful (e.g. the button has been clicked) is a sign of good UX. Therefore we should always look for embracing `:hover` and `:active` pseudo-classes.

#### Outline

Outline is a crucial element when it comes to website accessibility. Even though sometimes it's visually disturbing and unnecessary, we should never remove outline for keyboard users. There's a Smart Outline library at [JS Utils](https://github.com/kollegorna/js-utils) that helps to deal with the issue:

```js
  initSmartOutline()
```
This inits Smart Outline which hides the outline when interacting with a mouse and brings it back when interacting with a keyboard.

In some cases it's really meaningful to reveal the outline for mouse users as well. For example, let's say there is a confirm-type modal that pops up with two buttons ("Delete" and "Cancel"). Adding the focus on the primary action button and revealing the outline would tell the user they can also press "Enter" button to delete an item, e.g.:

```js
  confirmModal.on('show', (confirmBtn, cancelBtn) => {
    confirmBtn.focus()
    showSmartOutline()
  })
```

### Performance

* If possible, do not transition `top/bottom/left/right` properties, use `transform: translate()` instead;
* Accelerate "expensive" CSS solutions with [`will-change`](https://developer.mozilla.org/en-US/docs/Web/CSS/will-change), but do not overuse it.

### Fonts

* When using webfonts, always be sure to set [web-safe fallback font](https://www.cssfontstack.com) names. Our [SASS-Boilerplate](https://github.com/kollegorna/sass-boilerplate) comes with pre-defined web-safe fonts.

    ```scss
    $ff-primary: "proxima-nova", "Arial", "Helvetica Neue", "Helvetica", sans-serif;
    ```

* Often times when using multiple typefaces on a project they have different font weights. Usually the default font weight for the document is `400`. However, it happens a lot that the secondary font doesn't have `400` available as font weight by design. It means that each time you set the secondary font for a component, you also have to specify the font weight in order preserve the visual consistency. Our [SASS-Boilerplate](https://github.com/kollegorna/sass-boilerplate) comes with predefined mixins that automates the process, e.g.:

    ```scss
    .btn {
      @include ff-secondary;
    }

    // ...becomes:

    .btn {
      font-family: "SecondaryFontName", "Arial", sans-serif;
      font-weight: 500;
    }
    ```

### Vendor Prefixes

Vendor prefixed properties should automatically be inserted by an asset bundlers, therefore it's best to keep your code clean and use only prefix-free properties. This doesn't apply to properties that doesn't have standardized equivalents (such as `-webkit-overflow-scrolling`) or work differently than their standardized equivalents.

### Design Systems

It's common for us to work on several different projects for the same client. When this happens, we've found it useful to develop a collection of global, reusable styles — which we call a Design System. When declared as a dependency on a project, a design system gives us a nice collection of sensible defaults we can use to get started faster. Should the need arise, it also lets us update one default style across all projects related to a specific client. If you believe a design system would be beneficial in the long run, there is [a boilerplate with some sensible defaults in place](https://github.com/kollegorna/design-system-boilerplate). The repo includes instructions on how to set it up, as well as recommendations on how to seamlessly include it in your project without disrupting your workflow.

### Resources

* https://github.com/kollegorna/sass-boilerplate
* https://github.com/kollegorna/sass-utils
* https://github.com/airbnb/css#oocss-and-bem
* http://getbem.com/introduction
* http://www.intelligiblebabble.com/a-pattern-for-writing-css-to-scale
* https://zellwk.com/blog/media-query-units
* https://developer.mozilla.org/en-US/docs/Web/CSS/will-change
* https://www.cssfontstack.com

**[🚡 back to top](#table-of-contents)**

## JavaScript

### Style

We use [Airbnb's JS style guide](https://github.com/airbnb/javascript) as reference.

### ES6

We prefer using ES6 together with [Babel](https://babeljs.io), to ensure the code is compiled down into browser-compatible JavaScript.

### Linter

We recommend using [ES lint](http://eslint.org) with [this config](https://github.com/kollegorna/frontend-manual/tree/master/examples/eslint-config.json).

### JS Utils

We maintain and use [JS Utils](https://github.com/kollegorna/js-utils) library on real-life projects for easier development. The code examples below also rely on the library.

### jQuery

We discourage using jQuery for new projects if possible. Instead, strive to rely on dependency-free lightweight libraries, such as JS Utils and [similar ones](https://github.com/kollegorna/js-utils#other-resources) or consider [conditional loading](#loading-large-libraries-conditionally) for jQuery and its plugins.

### Selecting DOM elements

Even though ID attribute was primarily designed as an accessibility feature for fragmenting document, but the requirement for uniqueness, specificity, componentization are the actual reasons why ID's for selecting DOM elements in JS should be avoided by any means.

### Progressive Enhancement, Graceful Degradation

Treating JavaScript as an ehancement allows to code **fail-safe**, semantic and accessible websites. Take a look at the "Recent news" list example:

  ```html
  <h3>Recent news</h3>
  <ul class="recent-news">
    <li>...</li>
    <li>...</li>
    <!-- ... -->
    <li class="--hidden">...</li>
    <li class="--hidden">...</li>
    <!-- ... -->
  </ul>
  <a href="/more-posts" class="js--show-more">Show more</a>
  ```
  ```js
  const btn = document.querySelector('.js--show-more')
  btn.setAttribute('role', 'button')
  addEventListener(btn, 'click', (e) => {
    const hiddenItems = document.querySelectorAll('.recent-news li.--hidden')
    if(hiddenItems.length) {
      e.preventDefault()
      btn.removeAttribute('role')
      removeClass(hiddenItems, '--hidden')
    }
  })
  ```

Instead of assuming JavaScript is there and using `button`, we use `a[href]` which would redirect users to the corresponding page in case if JavaScript is disabled, it failed or hasn't been loaded yet. In parallel we also progressively enhance the experience with some JavasScript which turns the anchor into a semantic button `a[role=button]`. On the first click it reveals the hidden items and on the second it works as a typical anchor that redirects users to the corresponding page.

### Accessibility

* When working with elements users are supposed to press on, use HTML tags meant for that purpose:

    ```js
    addEventListener('.btn', 'click', doSomething)
    ```

    ✅ DO:
    ```html
    <button class="btn" type="button">Go baby!</button>
    ```

    ❌ DON'T:
    ```html
    <div class="btn">Go baby!</div>
    <a class="btn">Go baby!</a> <!-- <a> without [href] is inaccessible -->
    ```

* When crafting rich (JS enhanced) UI, always make sure it is usable by keyboard and screen readers:
  - it embraces [ARIA](https://developers.google.com/web/fundamentals/accessibility/semantics-aria) attribures
  - visually hidden elements are excluded from tab order
  - the rest of the page is excluded from tab order when modal is opened
  - dropdown menus and tabs work as least with Tab button (enabling arrow buttons would be a nice touch)

### Performance and Optimization

#### Embedding JavaScript

Avoid placing JS file insertions in <head> that work in a synchronous manner and therefore blocks rendering of the page. The more such an occasions, the later users start seeing the page. JavaScript file embeds should be placed right before </body> tag preferably with [`defer` or `async`](http://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html) attributes inserted along.

```html
<!DOCTYPE html>
<html>
  <head>
    ...
  </head>
  <body>
    ...
    <script src="/main.js" defer></script>
  </body>
</html>
```

In cases (e.g. Modernizr) when there's a need to run some JavaScript code in `<head></head>` before page renders, it's best to minify and inline it:

```html
<head>
  ...
  <script>
    (function(){ /*...*/ })();
  </script>
</head>
```

Even though ideally all the JavaScript code should be placed in external files, it's sometimes okay to inline it at the end of the document, preferably after external JS file references, e.g.:

```html
  <!-- ... -->
  <script src="/main.js" defer></script>
  <script>
    // inlined JS
  </script>
</body>
</html>
```

Avoid inlining JavaScript everywhere else.

#### Throttling and Debouncing

Attaching "heavy" functions to scroll, resize events can be cause of [unresponsive pages, even browser crashes](https://css-tricks.com/debouncing-throttling-explained-examples), excessive Ajax requests, etc. By default, always use handler [throttling] and [deboucing] for `scroll`, `resize` events:

```js
addEventListener(window, 'resize', debounce(500, () => {
  // do something expensive here
}))

addEventListener(window, 'scroll', throttle(500, () => {
  // do something expensive here
}))

// minimize Ajax requests for repetitive cart quantity button clicks
addEventListener(qtyBtn, 'click', debounce(300, ajaxCartQtyChange))
```

#### Passive Event Listeners

Use passive event listeners whenever possible – this can [improve scrolling performance](https://developers.google.com/web/tools/lighthouse/audits/passive-event-listeners).

```js
const doesSomethingNice = () => {
  // do something nice here
}

addEventListener(el, 'touchstart', doesSomethingNice, {passive: true})
```

#### Loading Large Libraries Conditionally

Consider loading large JavaScript libraries conditionally rather than bundling them in to the main file when you need them very rarely on a website, e.g.: you have a chart only on "about" page:

```js
const chart = document.querySelector('.about-chart')
if(chart) {
  loadScript('chart-lib.js').then(() => {
    ChartLibObj.create(chart)
  })
}
```

...or you must use a plugin with jQuery dependency while your whole website is jQuery-independent:

```js
const list = document.querySelector('.sortable-list')
if(list) {
  serialPromises(
    () => loadScript('jquery.min.js'), // best if from CDN
    () => loadScript('jquery-ui.min.js'), // best if from CDN
  ).then(() => {
    $(list).sortable()
  })
}
```

#### Performant and Tidy jQuery Code

In case you still have to use jQuery use it wisely. jQuery selectors should be cached into variables both for optimization reason and better code readability. Variable names that hold a jQuery object value should be prefixed with `$` sign.

✅ DO:

```js
var $win = $(window);
$win.on('scroll', ...)
$win.on('resize', ...)

$('.items', function() {
  var $item = $(this);
  $item.attr(...)
  $item.addClass(...)
  $item.on(...)
});
```

❌ DON'T:

```js
$(window).on('scroll', ...)
$(window).on('resize', ...)

$('.items', function() {
  $(this).attr(...)
  $(this).addClass(...)
  $(this).on(...)
});
```

Also be sure to check [jQuery performance guide](https://learn.jquery.com/performance).

### Animations

Our suggestion is not to rely on JavaScript for animations or transitions, if the same effects can be accomplished purely with CSS (using JavaScript for class toggling only).

### Resources

* https://github.com/kollegorna/js-utils
* https://developers.google.com/web/fundamentals/accessibility/semantics-aria
* https://es6.io
* https://css-tricks.com/debouncing-throttling-explained-examples

**[🚡 back to top](#table-of-contents)**

## Media

### Vector Images (SVG)

We have a three-pronged approach at using vector images:

* **`<use>`** for customisable (UI-related, decorational) elements
* **`<img>`** for static images (part of the content)
* **`<svg>`** (inline) for special situations.

#### `<use>`

Whenever you want your SVG's paths' colours to be customisable through CSS, this is the way to go. The technique is mostly enough for embedding decorational UI graphics like icons. It's performant and accessible, but still gives us some room for customisation. Please keep in mind, however, that this only allows you to choose a fill colour for all paths in a given group (styling different paths with different colours, or setting fills and strokes, for example, won't work).

When using this approach, SVG code can go inside one or several .svg files wrapped in a `<symbol>` tag:

```html
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">

  <symbol id="menu" viewBox="0 0 40 40">
    <title>menu</title>
    <path d="M0 0v89.1l22.3-22.3 18.6 37.2h7.4s4..."/>
  </symbol>

  <symbol id="close" viewBox="0 0 40 40">
    <!-- ... -->
  </symbol>

  <!-- ... -->

</svg>
```

You should then embed the graphics into HTML as follows:

```html
<svg><use xlink:href="/icons.svg#menu"></use></svg>
```

Make sure your `<svg>` instances are accessible (https://css-tricks.com/accessible-svgs/#article-header-id-8): either tell the screen readers to ignore the element (`[aria-hidden=true]`) or add a title (`<title> or [aria-label=”...”]`).

#### `<img>`

When adding SVGs as static images (e.g. illustrations) as part of the content you can embed them directly with the `<img>` tag, as you would any other image. Remember to fill in the `alt` attribute for accessibility reasons.

#### `<svg>` (inline)

Whenever complex styling is required, you can use inline SVGs. However, because this is less accessible and performant, it should not be used whenever any of the two solutions above are viable options.

Make sure your `<svg>` instances are accessible (https://css-tricks.com/accessible-svgs/#article-header-id-8): either tell the screen readers to ignore the element (`[aria-hidden=true]`) or add a title (`<title> or [aria-label=”...”]`).

When going with this approach, we should refrain from pasting the SVG code directly on the page and use a library to handle the embedding for us. You can use any libraries you want, but we suggest these two:

* Ruby: [inline_svg](https://github.com/jamesmartin/inline_svg)
* JavaScript: [SVGInjector](https://github.com/iconic/SVGInjector)

#### Animations

Animating SVGs through CSS or SMIL should be approached with caution. Browser support for CSS Animations is buggy and not wide enough yet, and SMIL is not supported in IE/Edge and will soon be deprecated everywhere else. For the time being, we suggest you use JavaScript libraries such as [Snap.svg](http://snapsvg.io) or [Velocity.js](http://velocityjs.org).

### Raster

For raster images, we should use JPGs when the image's contents are mostly photographic in nature (i.e. where colour clustering is unlikely to be noticeable) and PNGs when the image is mostly geometric, has large homogeneous swaths of colour or when transparency is required.

### Icons

If we're using an existing icon library available as an icon font, we should use that and make any necessary adjustments (e.g. add missing icons or tweak existing ones). If we're using custom icons, or icons from various different sources (e.g. The Noun Project), we should use SVGs. Using raster file types (such as PNGs) for icons is strongly discouraged.

### Screen Sizes and Pixel Density

We use Lazysizes to serve images for different screen sizes and pixel densities: https://github.com/aFarkas/lazysizes

### Optimisation

Images should always be optimised before the site goes live. This should be done in a non-destructive manner (i.e. you should make sure the original, non-compressed images are still easily available somewhere). For JPGs, most software has decent compression algorithms. For PNGs, you'll find a list of useful optimisation resources below.

### Resources

#### Tools

* http://nukesaq88.github.io/Pngyu
* https://imageoptim.com (https://imageoptim.com)
* https://tinypng.com/ (for both PNG and JPG)

#### Tutorials

* https://css-tricks.com/accessible-svgs

**[🚡 back to top](#table-of-contents)**

## Structured Data

Use of JSON-LD and Schema.org (http://schema.org) definitions to markup content is strongly encouraged whenever its benefits for the project are self-evident. You should use your best judgement in assessing whether or not the benefits justify the time invested in properly structuring data.

### Resources

#### Learn More

* https://developers.google.com/search/docs/guides/intro-structured-data

**[🚡 back to top](#table-of-contents)**

## Libraries

See [Libraries.md](Libraries.md)

**[🚡 back to top](#table-of-contents)**

## Accessibility

Building applications and websites that are usable for as many people (and bots) as possible is in our backbone. It's not some extra topping on the ice cream but rather something that should be considered during the entire process of a project.

### WCAG 2.0 Level AA

Code should be compliant with WCAG 2.0 Level AA. This is something that needs to be considered in the design process as well as one of the ingredients in the WCAG mix is color contrast.

### Resources

#### Learn More

* http://webaim.org/intro (required reading)
* https://madebysidecar.com/journal/accessibility-basics-for-designers
* http://webaim.org/standards/wcag/checklist
* https://developer.mozilla.org/en-US/docs/Web/Accessibility
* https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA
* https://webbriktlinjer.se/r/1-utga-fran-wcag-2-0-niva-aa (Swedish)

#### Docs & specs

* http://checkers.eiii.eu/en/pagecheck2.0
* http://achecker.ca/checker
* http://cynthiasays.com
* http://www.tawdis.net/ingles.html?lang=en

#### Tools

* http://webaim.org/resources/contrastchecker

#### Suggestions

* Whenever possible, choose frameworks and libraries that are accessible by default. Foundation for example has accessibility in its backbone (http://foundation.zurb.com/sites/docs/accessibility.html).
* Make sure ARIA tags are used appropriately. Consider using JavaScript to add them (https://codepen.io/dencarlsson/pen/vmaqNQ).

**[🚡 back to top](#table-of-contents)**

## Performance

### Tools

* https://developers.google.com/speed/pagespeed/
* https://tools.pingdom.com (only on live sites since the last checked sites are displayed on the page… don't spill any secrets)

**[🚡 back to top](#table-of-contents)**

## Support and Compatibility

See [Support and Compatibility](Support and Compatibility.md)

**[🚡 back to top](#table-of-contents)**
