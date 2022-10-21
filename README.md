# Odd Camp's Front End Manual
_v0.7_

This document outlines the basic stuff for Front End development at [Odd Camp](https://www.odd.camp). We should try to keep this as tiny as possible and include only the most important stuff.

These things are the default for all our projects unless anything else is specifically said. The entire Front End team should know these things. If you have questions or suggestions regarding this manual, hit up [Osvaldas](https://github.com/osvaldasvalutis). If you feel that you need some time of powering up your skills just holla at [Per](https://github.com/persand) and you'll get it.

## Table of Contents

- [Code editor](#code-editor)
- [Project setup](#project-setup)
- [HTML](#html)
- [CSS](#css)
- [JavaScript](#javascript)
- [Media](#media)
- [Accessibility](#accessibility)
- [Performance](#performance)
- [Compatibility](#compatibility)
- [Design](#design)

## Code editor

We usually use linters and [.editorconfig](http://editorconfig.org/#download) in our projects. In order to smoothen the development process install ESlint and Stylelint extensions in your code editor. For _Visual Studio Code_ use these:

* [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
* [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
* [stylelint](https://marketplace.visualstudio.com/items?itemName=stylelint.vscode-stylelint)
* [EditorConfig for VS Code](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)

[🚡 back to top](#table-of-contents)

## Project Setup

### Starters and boilerplates

When starting a Rails or GatsbyJS project, we usually use these pre-configured starters/boilerplates:

* [gatsby-starter-oddcamp](https://github.com/oddcamp/gatsby-starter-oddcamp)
* [rails-boilerplate](https://github.com/oddcamp/rails-boilerplate)

If for some reason you need to start a project from scratch, please make sure to adapt as much of the following as possible:

### Dependencies

Use Yarn for dependencies:

* `yarn init`
* `yarn add package-name`

### .editorconfig

All projects must have an [.editorconfig file](examples/.editorconfig) by default.

### Linters

Set up [ESLint](http://eslint.org), [stylelint](https://stylelint.io) and [Prettier](https://prettier.io) using the following configurations:

* [ESLint config](https://github.com/oddcamp/frontend-manual/tree/master/examples/.eslintrc)
* [stylelint config](https://github.com/oddcamp/frontend-manual/blob/master/examples/.stylelintrc)
* [Prettier config](https://github.com/oddcamp/frontend-manual/tree/master/examples/.prettierrc)

[🚡 back to top](#table-of-contents)

## HTML

### Semantics and Accessibility

We should make an effort to produce valid [semantic HTML](https://codepen.io/mi-lee/post/an-overview-of-html5-semantics), that takes advantage of the full potential of HTML5's tags to produce clean code, that is readable by humans and machines alike.

> Don't mark the content up by how it looks, mark it up by what it means.

* Use available tags (`header`, `section`, `article`, etc.) to section content by its meaning, e.g.:

    ✅ DO:
    ```html
    <section>
      <h1>Articles</h1>

      <article>
        <h2>Title</h2>
        <p>Excerpt</p>
      </article>

      <article>
        <h2>Title</h2>
        <p>Excerpt</p>
      </article>
    </section>
    ```

    ❌ DON'T:
    ```html
    <div>
      <div>Articles</div>

      <div>
        <div>Title</div>
        <div>Excerpt</div>
      </div>

      <div>
        <h2>Title</div>
        <div>Excerpt</div>
      </div>
    </div>
    ```

* An element that looks like a heading isn't necessarily a heading semantically, e.g.:

    ✅ DO:
    ```html
    <h1>Title</h1>
    <p class="styled-h2">A paragraph that looks like `h2` in the design</p>
    ```

    ❌ DON'T:
    ```html
    <h1>Title</h1>
    <h2>A paragraph that looks like `h2` in the design</h2>
    ```

* Use `<time>` for marking dates:

    ✅ DO:
    ```html
    <time datetime="2001-05-15T19:00">May 15, 2001</time>
    ```

    ❌ DON'T:
    ```html
    <span>May 15, 2001</span>
    ```

* Use list tags `ul`, `ol`, `dl` to structure lists, e.g.:

    ✅ DO:
    ```html
    <dl>
      <dt>Title</dt>
      <dd>Description</dd>

      <dt>Title</dt>
      <dd>Description</dd>
    </dl>
    ```

    ❌ DON'T:
    ```html
    <div>
      <div><b>Title</b></div>
      <div>Description</div>

      <div><b>Title</b></div>
      <div>Description</div>
    </div>
    ```

* When working with elements users are supposed to interact with, use HTML tags meant for that purpose, e.g.:

    ✅ DO:
    ```html
    <button class="btn" type="button">Go baby!</button>
    ```

    ❌ DON'T:
    ```html
    <div class="btn">Go baby!</div> <!-- not focusable with keyboard, etc. -->

    <a class="btn">Go baby!</a> <!-- <a> without [href] is inaccessible -->
    ```

* Exclude repeating links and controls from tab order, e.g.:

    ✅ DO:
    ```html
    <article>
      <a href="/same-url" tabindex="-1">
        <img src="..." alt="..." />
      </a>

      <h2><a href="/same-url">...</a></h2>
    </article>
    ```

* Provide text for links, buttons and images, otherwise screen readers will read full URLs for the users. Also use `[title]` attributes as hints for mouse users:

    ✅ DO:
    ```html
    <a href="/" class="-image" aria-label="Clickable image"></a>

    <button class="-next" type="button" title="Next"><svg aria-title="Next">...</svg></button>

    <img src="..." alt="Description" />
    ```

    ❌ DON'T:
    ```html
    <a href="/" class="-image"></a>

    <button class="-next"><svg>...</svg></button>

    <img src="..." />
    ```

* Forms should have fragment identifiers set properly, which is crucial if the form appears _below the fold_, e.g.:
    ✅ DO:
    ```html
    <form method="post" action="/contact#add-contact-form" id="add-contact-form">...</form>
    ```

* Also make sure:
  - it embraces [ARIA](https://developers.google.com/web/fundamentals/accessibility/semantics-aria) attributes
  - UI's are usable with a keyboard
  - visually hidden elements are excluded from the tab order
  - the rest of the page is excluded from tab order when a modal is opened
  - dropdown menus and tabs work at least with the Tab key (enabling arrow buttons would be a nice touch)
  - click and touch targets are at least `44x44px` in size

### Smart Quotes

Use the [correct quotation marks and apostrophes](http://smartquotesforsmartpeople.com) for content:

  * ❌ "Don't be dumb"
  * 👌 “You’re smart!”

### Minimum viable `<head>` tag composition

We recommend using [these tags](https://github.com/oddcamp/frontend-manual/tree/master/examples/head-tags-recomended.html) (as well as [manifest.json](https://github.com/oddcamp/frontend-manual/tree/master/examples/manifest.json)) in the HEAD area are of the document as a starting point.

### Templating languages

When writing HTML code in an environment that includes a templating engine (be it ERB or JSX), it's very easy to mess up code readability. In order to avoid that, it's best to put as much logic into the controllers as possible. If you still have to write framework related code in a template, it's better to do as much of it at the top of the file as possible, e.g.:

✅ DO:
```erb
<%
  people = WPRecord.by_model("person").where(uid: owners)
%>

<ul>
  <% people.each do |person| %>
    <li>
      <%= person[:fullname] %>
    </li>
  <% end %>
</ul>
```

❌ DON'T:
```html
<ul>
  <% WPRecord.by_model("person").where(uid: owners).each do |person| %>
    <li>
      <%= person[:fullname] %>
    </li>
  <% end %>
</ul>
```

### Learn More

* https://codepen.io/mi-lee/post/an-overview-of-html5-semantics
* http://smartquotesforsmartpeople.com

[🚡 back to top](#table-of-contents)

## CSS

### Preprocessor

* We use [SASS](https://sass-lang.com) as the standard for our styling needs.
* We use [Styled Components](https://www.styled-components.com) and [Polished](https://polished.js.org) for JS frameworks based projects.

### File Structure and Boilerplate

We usually use [SASS-Boilerplate](https://github.com/oddcamp/sass-boilerplate) together with [SASS-Utils](https://github.com/oddcamp/sass-utils) for SASS projects (please follow the guidelines provided in the repository pages).

- Aim for **componentization**, i.e. creating multiple independent/encapsulated small structures rather than a few large ones.

- In SASS projects, we usually split components into two categories/folders:
  - `components` – global components
  - `pages` – page-specific components

- Be very strict at placing components in their own files, e.g.:

  ✅ DO:
  ```scss
  // _button.scss
  .button {
    // ...
  }

  .button-special {
    // ...
  }
  ```

  ❌ DON'T:
  ```scss
  // _button.scss
  .button {
    // ...
  }

  .checkbox {
    // ...
  }
  ```

### Naming

* We use [BEM](http://getbem.com) for naming convention.

* Selectors shouldn't be chained above three levels:
    * 👌 `.settings-nav` is good
    * 👌 `.settings-nav__links` is good
    * ✅ `.settings-nav__links__list` is ok
    * ❌ `.settings-nav__links__list__item` should be avoided

* In case you need to chain more than three selector levels, consider nesting. Nested single class names should start with a dash, which indicates that this selector is scoped/local (strictly belongs to the parent selector) and altogether it won't conflict with global components that might habe the same name:
    * 👌 `.settings-nav .-links` is good
    * ❌ `.settings-nav .links` should be avoided

    **Important!** Do not confuse the last example with cases where you are extending a global component:

    * 👌 `.settings-nav .links` is good if `.links` was a global component and was meant to be extended under the `.settings-nav` component

* For performance reasons, selectors shouldn't nest more than three levels deep:
    * 👌 `.settings-nav .-links__list` is good
    * 👌 `.settings-nav .-links .-list` is good
    * ❌ `.settings-nav .-links .-list .-item` should be avoided

    They also shouldn't nest when there's no reason to:
    * 👌 `.settings-nav ul a` is good
    * ❌ `.settings-nav ul li a` should be avoided

* For secure scoping reasons, modifier classes should be formated according to BEM, or concatenated with double-dashed class names:
    * 👌 `.settings-nav--hidden` is good
    * ✅ `.settings-nav.--hidden` is ok
    * ❌ `.settings-nav.hidden` should be avoided

### Using ID's

Although the ID attribute was primarily designed as an accessibility feature for fragmenting document, the requirements for uniqueness, specificity, and componentization are incompatible with its use in CSS. We should avoid using ID's, except in cases where they are strictly required (e.g. to uniquely identify a specific instance of a component, to set page anchors, among others).

### Componentization

Treating the whole page as a single component can easily get you in selector-chain hell. It becomes difficult to use parent modifier classes that affect child elements, and your code quickly turns into spaghetti. Therefore, it's recommended to always treat the page as a combination of multiple and small components, which by their nature are easily reusable and extendable, making the code more intelligible.

❌ Avoid complex SASS structures like this:

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

If you have a component that is reused multiple times on the same page, avoid making assumptions about its context (i.e. use positioning-related properties such as `margin`, `position/top/left/...`). Instead, put that responsibility on a parent component:

```scss
.settings-avatar {
  // ...
}

// avatar in sidebar
.settings-sidebar {
  position: relative;

  .settings-avatar {
    position: absolute;
    top: 0;
    left: 0;
  }
}

// avatar in main container
.settings-main {
  // ...

  .settings-avatar {
    margin-top: 1.5rem;
  }
}
```

### Extensions and overrides

It's usually a better practice to create extensions for a nested component, rather than overriding it. That way, things are more predictable and controllable:

✅ DO:
```scss
.button {
  &--green {
    color: $color-green;
  }
}
```

❌ DON'T:
```scss
.settings-sidebar {
  .button {
    color: $color-green;
  }
}
```

However, if you need to adjust how the nested component behaves in a specific context, _overriding_ makes sense:

✅ DO:
```scss
.settings-sidebar {
  .button {
    margin-top: rem(20);
  }
}
```

### Units

For better accessibility we should use EMs/REMs and set the font size of the root element (`html`) to a percentage value, preferably `100%`:

```scss
html {
  font-size: 100%;
}
```

This enables the users (most likely visually impaired) to scale the site visually. For example: let's say the root element's font size on our site is as recommended — `100%`. By default the website font size in the browser's settings is set to 16px, which means `1rem = 16px`. But if the user has that option set to `32px` the REM value is going to be affected accordingly: `1rem = 32px`. Accessible!

REMs should be used by default, EMs when we need local dependencies, and PXs only for the rare cases when things aren't supposed to scale at all (e.g. 1px thick borders). EMs and REMs should be calculated through a [helper from SASS-Utils](https://github.com/oddcamp/sass-utils#units) or [Polished](https://polished.js.org/docs/#rem):

```scss
.component {
  width: em(320); // 320px
  padding: rem(30 20); // 30px 20px
  margin: rem(40 auto 20); // 40px auto
}
```

Using these _proportional_ units enables UI to depend on the screen size, e.g.:

```scss
html {
  font-size: 100%;

  @include mq(medium down) {
    font-size: 87.5%;
  }
}
```

### Variables

_Variablize_ as many of the global configurations as possible. Our [SASS-Boilerplate](https://github.com/oddcamp/sass-boilerplate/tree/master/src/base) will get you on the way.

Don't forget to set global `z-index`'es as variables – this will save you some time dealing with scroll fixed headers, modals, and such.

**Important:** Media Queries have to be EMs based ([here's why](https://zellwk.com/blog/media-query-units)). This is already solved by a helper from [SASS-Utils](https://github.com/oddcamp/sass-utils#mq-mixin).

### "Styled" Strategy

_Styled_ is a strategy for styling HTML elements that are usually inserted via WYSIWYG editors when writing articles, blog posts, etc. (`h1-6, p, blockquote, a, ul, ol, dl, table, code, pre`, among other common tags). When starting a new project we prefer every HTML element to be unstyled (naked) by default, and unopinionated about the context it's in. Benefits are:

- No need to overwrite default styles (e.g. remove margins, change hover effects, etc.) when an element is in a different context or should be styled differently;
- No need to track the changes in the default styling of an element and update every single instance where the styling was meant to be completely different;
- Visual consistency among browsers;
- Smaller CSS file size;
- **Always be sure** you attach the major `styled` class name to the direct parent element of the content:

  ✅ DO:
  ```html
  <div class="settings-main">
    <div class="settings-content styled"> <!-- 👈 this is correct -->
      <p>...</p>
      ...
    </div>
  </div>
  ```

  ❌ DON'T:
  ```html
  <div class="settings-main styled">
    <div class="settings-content">
      <p>...</p>
      ...
    </div>
  </div>
  ```
- Never `@extend .styled` as it will unnecessarily increase the size of the CSS bundle.

For further details, usage and tips follow the ["Styled" guide on SASS-boilerplate](https://github.com/oddcamp/sass-boilerplate#styled-strategy) repository page.

### Responsive Breakpoints

You should structure breakpoints to avoid overrides: define shared styles first and put the rest inside appropriate media queries.

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
* No need to overwrite styles in a media query when changing or adding the default ones;
* Smaller CSS file size.

We use [Media Queries helper](https://github.com/oddcamp/sass-utils#media-queries) from our SASS-Utils library.

### Usability and Accessibility

#### Indicating Interaction

Visually indicating that an element is available to be interacted with (e.g. a button is clickable) or the interaction has been successful (e.g. the button has been clicked) is a sign of good UX. Therefore we should always look to embrace `:hover`, `:focus` and `:active` pseudo-classes.

#### Outline

Outline is a crucial element when it comes to website accessibility. Even though sometimes it's visually disturbing and unnecessary, we should never remove outline for keyboard users. There's a Smart Outline library at [JS Utils](https://github.com/oddcamp/js-utils) that helps to deal with the issue:

```js
initSmartOutline()
```
Smart Outline hides the outline when interacting with a mouse and brings it back when interacting with a keyboard.

In some cases, it's really meaningful to reveal the outline for mouse users as well. For example, let's say there is a confirm-type modal that pops up with two buttons ("Delete" and "Cancel"). Adding the focus on the primary action button and revealing the outline would tell the user they can also press "Enter" button to delete an item, e.g.:

```js
confirmModal.on('show', (confirmBtn, cancelBtn) => {
  confirmBtn.focus()
  showSmartOutline()
})
```

### Performance

* If possible, do not transition `top/bottom/left/right` properties, use `transform: translate()` instead;
* Accelerate "expensive" CSS solutions with [`will-change`](https://developer.mozilla.org/en-US/docs/Web/CSS/will-change), but do not overuse it;
* Avoid nesting more than three selectors. Proper componentisation will help you;
* Don't hide text with `text-indent: -9999px`, there is a [better technique for it](http://www.zeldman.com/2012/03/01/replacing-the-9999px-hack-new-image-replacement/);
* To make objects round, do `border-radius: 50%` instead of `999px`.

### Fonts

* When using webfonts, always be sure to set [web-safe fallback font](https://www.cssfontstack.com) names. Make sure the fallback fonts are as close to the original one as possible. Our [SASS-Boilerplate](https://github.com/oddcamp/sass-boilerplate) comes with pre-defined web-safe fonts.

    ```scss
    $ff-primary: "proxima-nova", "Arial", "Helvetica Neue", "Helvetica", sans-serif;
    ```

* Oftentimes, when using multiple typefaces on a project, they have different font weights. Usually, the default font-weight for the document is `400`. However, it will sometimes happens that the secondary font doesn't have `400` available as font-weight by design. It means that each time you set the secondary font for a component, you also have to specify the font-weight in order to preserve the visual consistency. Our [SASS-Boilerplate](https://github.com/oddcamp/sass-boilerplate) comes with predefined mixins that automate the process, e.g.:

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

Vendor prefixed properties should automatically be inserted by an asset bundler, therefore it's best to keep your code free of prefixed properties. This doesn't apply to properties that don't have standardized equivalents (such as `-webkit-overflow-scrolling`) or work differently than their standardized equivalents.

### Design Systems

It's common for us to work on several different projects for the same client. When this happens, we've found it useful to develop a collection of global, reusable styles — which we call a Design System. When declared as a dependency on a project, a design system gives us a nice collection of sensible defaults we can use to get started faster. Should the need arise, it also lets us update one default style across all projects related to a specific client. If you believe a design system would be beneficial in the long run, there is [a boilerplate with some sensible defaults in place](https://github.com/oddcamp/design-system-boilerplate). The repo includes instructions on how to set it up, as well as recommendations on how to seamlessly include it in your project without disrupting your workflow.

### Also...
- **Always make sure that any images, videos, and iframes have dimensional properties defined in CSS.** Never depend on the size of the original source as this is prone to change and break the layout.

  ✅ DO:
  ```scss
  svg {
    width: em(20);
    height: em(20);
    opacity: 0.8;
  }

  img {
    max-width: em(240);
    width: 100%;
  }
  ```

  ❌ DON'T:
  ```scss
  svg {
    opacity: 0.8;
  }

  img {
    max-width: em(240);
  }
  ```
- Never use CSS's property `content` for text – it's not an accessible solution:

  ❌ DON'T:
  ```scss
  button::after {
    content: "Display options"
  }
  ```
- Use relative values for `line-height`s rather than fixed:

  ✅ DO:
  ```scss
  p {
    line-height: 1.2;
  }
  ```

  ❌ DON'T:
  ```scss
  p {
    line-height: 22px;
  }
  ```

### Resources

* https://github.com/oddcamp/sass-boilerplate
* https://github.com/oddcamp/sass-utils
* https://github.com/airbnb/css#oocss-and-bem
* http://getbem.com/introduction
* http://www.intelligiblebabble.com/a-pattern-for-writing-css-to-scale
* https://zellwk.com/blog/media-query-units
* https://developer.mozilla.org/en-US/docs/Web/CSS/will-change
* https://www.cssfontstack.com

[🚡 back to top](#table-of-contents)

## JavaScript

### Style

The way we format the code is dictated by [our linters setup](#linters), but [Airbnb's JS style guide](https://github.com/airbnb/javascript) should be used as a secondary reference source.

### ES6

We prefer using ES6 together with [Babel](https://babeljs.io), to ensure the code is compiled down into browser-compatible JavaScript.

### JS Utils

We maintain and use [JS Utils](https://github.com/oddcamp/js-utils) library on real-life projects for easier development. The code examples below also rely on the library.

### jQuery

We discourage using jQuery for new projects if possible. Instead, strive to rely on dependency-free lightweight libraries, such as JS Utils and [similar ones](https://github.com/oddcamp/js-utils#other-resources) or consider [conditional loading](#loading-large-libraries-conditionally) for jQuery and its plugins.

### Selecting DOM elements

* Always be specific about what you are selecting, in order to avoid unexpected outcomes:

  ✅ DO:
  ```js
  const settingsSidebar = document.querySelector(`.settings-sidebar`)
  const toggles = settingsSidebar.querySelectorAll(`input[type="radio"]`) // 👈 this is correct
  ```

  ❌ DON'T:
  ```js
  const settingsSidebar = document.querySelector(`.settings-sidebar`)
  const toggle = document.querySelectorAll(`input[type="radio"]`)
  ```

  In this case, we specifically select inputs of the `settingsSidebar`, not the whole document.

### Progressive Enhancement, Graceful Degradation

Treating JavaScript as an ehancement promotes the development of **fail-safe**, semantic, accessible websites. Take a look at this "Recent news" list example:

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

Instead of assuming JavaScript is there and using `button`, we use `a[href]` which would redirect users to the corresponding page in cases where JavaScript is disabled, it failed, or it hasn't been loaded yet. In parallel, we also progressively enhance the experience with some JavasScript which turns the anchor into a semantic button `a[role=button]`. After the first click, it reveals the hidden items, and after the second it works like a typical anchor that redirects users to the corresponding page.

### Performance and Optimization

#### Embedding JavaScript

Avoid placing JS file insertions in <head> that work in a synchronous manner and therefore block rendering of the page. The more this happens, the later users will start seeing the page. JavaScript file embeds should be placed right before </body> tag, preferably with [`defer` or `async`](http://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html) attributes inserted along.

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

In cases when there's a need to run some JavaScript code in `<head></head>` before page renders (e.g. Modernizr), it's best to minify and inline it:

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

Attaching "heavy" functions to scroll, resize events can be the cause of [unresponsive pages, even browser crashes](https://css-tricks.com/debouncing-throttling-explained-examples), excessive Ajax requests, etc. By default, always use handler [throttling] and [deboucing] for `scroll`, `resize` events:

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
    window.ChartLibObj.create(chart)
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

In case you still have to use jQuery use it wisely. jQuery selectors should be cached into variables both for optimization reasons and better code readability. Variable names that hold a jQuery object value should be prefixed with `$` sign.

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

Our suggestion is not to rely on JavaScript for animations or transitions if the same effects can be accomplished purely with CSS (using JavaScript for class toggling only).

### Resources

* https://github.com/oddcamp/js-utils
* https://developers.google.com/web/fundamentals/accessibility/semantics-aria
* https://es6.io
* https://css-tricks.com/debouncing-throttling-explained-examples

[🚡 back to top](#table-of-contents)

## Media

### Vector Images (SVG)

We have a three-pronged approach to using vector images:

* `<use>` usually for monocolor icons
* `<svg>` (inline) for images whose look should be altered by CSS/JS of a page
* `<img>` for static images

#### `<use>`

Whenever you want your SVG's paths' colors to be customizable through CSS, this is the way to go. The technique is mostly enough for embedding decorational UI graphics like icons. It's performant and accessible, but still gives us some room for customization. Please keep in mind, however, that this only allows you to choose a fill color for all paths in a given group (styling different paths with different colors, or setting fills and strokes, for example, won't work).

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

Make sure your `<svg>` instances are [accessible](https://css-tricks.com/accessible-svgs/#article-header-id-8): depending on the context, either tell the screen readers to ignore the element (`[aria-hidden=true]`) or add a title (`<title> or [aria-label=”...”]`).

#### `<svg>` (inline)

Whenever complex styling is required, you can use inline SVGs. However, because this is less accessible and performant, it should not be used whenever any of the two solutions above are viable options.

#### `<img>`

When adding SVGs as static images (e.g. illustrations) as part of the content you can embed them directly with the `<img>` tag, as you would any other image. Remember to fill in the `alt` attribute for accessibility reasons.

#### Animations

We usually use CSS for basic animations and rely on JavaScript libraries such as [Snap.svg](http://snapsvg.io) or [Velocity.js](http://velocityjs.org) when implementing advanced animations.

### Icons

We prefer using SVG icons via `<use>` and/or inline `<svg>`. Using raster file types (such as PNGs) or fonts for icons is strongly discouraged.

In the case of monocolor SVG icons, their color should be alterable from the CSS of the page using `fill` property. For that to work the SVG code of an icon should have `viewBox` attribute set whereas color-defining attributes should be removed or replaced with `currentColor` value where needed.

### Raster images

For raster images, we should use JPGs when the image's contents are mostly photographic in nature (i.e. where color clustering is unlikely to be noticeable) and PNGs when the image is mostly geometric and has large homogeneous swaths of color or when transparency is required.

We should use `<picture>` and/or `[srcset]` as much as possible.

### Optimisation

Images should always be optimized before the site goes live. This should be done in a non-destructive manner (i.e. you should make sure the original, non-compressed images are still easily available somewhere). For JPGs, most software has decent compression algorithms. For PNGs, you'll find a list of useful optimization resources below.

### Resources

* https://css-tricks.com/accessible-svgs
* http://nukesaq88.github.io/Pngyu
* https://imageoptim.com (https://imageoptim.com)
* https://tinypng.com/ (for both PNG and JPG)

[🚡 back to top](#table-of-contents)

### Resources

#### Suggestions

* When using libraries in production, try to only require/import the pieces you need — e.g. if you're using Foundation's grid, only add Foundation's core, its grid component, and any dependencies it may have;
* When adding a library as a dependency, you should specify a version, to prevent builds from breaking with future updates;

**[🚡 back to top](#table-of-contents)**

## Accessibility

We've already touched some a11y cases, but building applications and websites that are usable for as many people (and bots) as possible is in our backbone. It's not some extra topping on the ice cream but rather something that should be considered during the entire process of a project.

### WCAG 2.1 Level AA

Code should be compliant with WCAG 2.1 Level AA. This is something that needs to be considered in the design process as well as one of the ingredients in the WCAG mix is color contrast.

### Resources

#### Learn More

* http://webaim.org/intro (required reading)
* https://madebysidecar.com/journal/accessibility-basics-for-designers
* http://webaim.org/standards/wcag/checklist
* https://developer.mozilla.org/en-US/docs/Web/Accessibility
* https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA
* https://webbriktlinjer.se/lagkrav/folj-standarder-tillganglighet/ (Swedish)

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

[🚡 back to top](#table-of-contents)

## Performance

The most important performance related info has been discussed in the manual. For more improvements ideas we always use [Lighthouse for Chrome extension](https://chrome.google.com/webstore/detail/lighthouse/blipmdconlkpinefehnmjammfjpmpbjk?hl=en) at the pre-launch stage of a product.

**[🚡 back to top](#table-of-contents)**

## Compatibility

While we are committed to ensuring the things we build can be used by as many users as possible, we also believe deep retroactive support hinders progress, so we try to make sure our work strikes the right balance of browser and device support.

For most projects, we should test exhaustively and make sure things work as expected in the following:

### Mobile Platforms
*Latest versions of…*

* iOS
* Android

### Mobile browsers
*Latest versions of…*

* Mobile Safari
* Chrome for Mobile

### Desktop Platforms
* Windows
* MacOS
* Linux

### Desktop browsers
*Two latest versions of…*

* Chrome
* Firefox
* Safari
* Edge
* Internet Explorer 11

This doesn't mean that it has to look exactly the same across different browsers. But rather that the overall functionality should work and that the content should be accessible.

[🚡 back to top](#table-of-contents)

## Design

We think adopting a few common sense practices during the early design stages can help prevent friction later on, when we get to code. So we suggest everyone follows a few simple procedures, to ensure the transition from design to code is as seamless and natural as possible.

### Software

You're free to use whatever software you want (including none at all) to design, but please be mindful of the impact your choices will have on the rest of the team. We try to use Sketch as much as possible for UI design, and it should be your preferred tool, as it's accessible to most of the team. Other software (e.g. Adobe CC, Affinity Designer, Principle) can still be used for other tasks, such as illustration, photo manipulation, motion graphics or print, but please make sure choosing it won't block someone else in the team from picking up where you left off.

### Typefaces

Choice of typefaces should be tailored for the project's needs. If doing work primarily for the Web, the typefaces should be optimised and license-able for Web use. We have accounts at several subscription services, so we recommend you browse and consider those first when choosing typefaces for a project. You'll find a list of these services, along with other relevant links, below.

Please be mindful of the impact Webfonts will have on the site's performance. Most services require loading additional external resources, which delays the site's first paint. If possible, try to choose typefaces that we can embed directly via `@font-face`.

### Presentation and Mock-ups

You're free to use whatever tools you prefer to present proposals and/or mock-ups to clients, but we recommend using InVision for prototyping UI work. It's a robust service, has an almost seamless integration with Sketch and has proved to be client-friendly. For other types of work, we're partial to using short and to-the-point PDF presentations.

### Design Files

As soon as the design work has been approved and it's moving to code, you should upload all of the original design files plus any necessary assets (e.g. fonts, icons, original images, etc) to Google Drive, under the client's folder (go to Drive > Clients and check for a folder with the client's name, or create one in case it doesn't exist). **This is required procedure, even for projects you're the only one working on**. Whenever there have been major updates to any of the design files or assets, you should re-upload them (Drive takes care of file history and versioning).

### Resources

#### Tools

* https://www.invisionapp.com
* https://rightfontapp.com

#### Typeface Services

* https://typekit.com/ (1)
* https://www.myfonts.com/info/mls
* https://fontstand.com
* https://www.fontspring.com/web-fonts
* https://www.typography.com/cloud/welcome (2)

(1) Login credentials on LastPass
(2) Legacy. Usage on new projects is discouraged

#### Suggestions

* Whenever possible, try to anticipate your colleagues' involvement in the project beforehand, and plan your software use accordingly.

[🚡 back to top](#table-of-contents)
