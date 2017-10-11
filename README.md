# Kollegorna's Front End Manual
###### v0.4

This document outlines the basic stuff for Front End development at Kollegorna. We should try to keep this as tiny as possible and include only the most important stuff.

These things are the default for all our projects unless anything else is specifically said. The entire Front End team should know these things. If you feel that you need some time of powering up your skills just holla at Per and you'll get it.

## Table of Contents

- [Setup](#setup)
  * [Add dependencies](#add-dependencies)
  * [Add .editorconfig](#add-editorconfig)
- [Design](#design)
  * [Software](#software)
  * [Typefaces](#typefaces)
  * [Presentation and Mock-ups](#presentation-and-mock-ups)
  * [Design Files](#design-files)
  * [Resources](#resources)
- [HTML](#html)
  * [Semantics](#semantics)
  * [Templating Languages](#templating-languages)
  * [Resources](#resources-1)
- [CSS](#css)
  * [Preprocessor](#preprocessor)
  * [Methodology](#methodology)
  * [Responsive Breakpoints](#responsive-breakpoints)
  * [File Structure](#file-structure)
  * [Design Systems](#design-systems)
  * [Resources](#resources-2)
- [JavaScript](#javascript)
  * [Style](#style)
  * [ES6](#es6)
  * [jQuery](#jquery)
  * [Routing](#routing)
  * [Resources](#resources-3)
- [Media](#media)
  * [Vector Images (SVG)](#vector-images--svg-)
  * [Raster](#raster)
  * [Icons](#icons)
  * [Screen Sizes and Pixel Density](#screen-sizes-and-pixel-density)
  * [Optimisation](#optimisation)
  * [Resources](#resources-4)
- [Structured Data](#structured-data)
  * [Resources](#resources-5)
- [Libraries](#libraries)
  * [Suggested Libraries](#suggested-libraries)
  * [Resources](#resources-6)
- [Accessibility](#accessibility)
  * [WCAG 2.0 Level AA](#wcag-20-level-aa)
  * [Best Practices](#best-practices)
  * [Resources](#resources-7)
- [Performance](#performance)
  * [Best Practices](#best-practices-1)
  * [Resources](#resources-8)
- [Support and Compatibility](#support-and-compatibility)
  * [Support Checklist](#support-checklist)
  * [Resources](#resources-9)
- [Tools](#tools-4)
  * [Task Runners](#task-runners)
  * [Dependency Managers](#dependency-managers)
  * [Linters](#linters)
- [Checklists](#checklists)
  * [Setup Checklist](#setup-checklist)
  * [Pre-Shipping Checklist](#pre-shipping-checklist)

## Setup

When starting a new project, make sure you do the following:

### Add dependencies

We expect certain dependencies to be bundled in by default with all of our projects. Below is a list of the libraries we recommend all projects include at setup. This list can be updated, so always refer back to it.

#### Yarn

* `yarn init`
* `yarn add normalize-scss`

### Add .editorconfig

All projects should have an [.editorconfig file](examples/.editorconfig) by default. If your editor doesn't have built-in editorconfig support, please [install the necessary plug-ins](http://editorconfig.org/#download).

**[🚡 back to top](#table-of-contents)**

## Design

We think adopting a few common sense practices during the early design stages can help prevent friction later on, when we get to code. So we suggest everyone follows a few simple procedures, to ensure the transition from design to code is as seamless and natural as possible.

### Software

You're free to use whatever software you want (including none at all) to design, but please be mindful of the impact your choices will have on the rest of the team. We try to use Sketch as much as possible for UI design, and it should be your preferred tool, as it's accessible to most of the team. Other software (e.g. Adobe CC, Affinity Designer, Principle) can still be used for other tasks, such as illustration, photo manipulation, motion graphics or print, but please make sure choosing it won't block someone else in the team from picking up where you left off.

### Typefaces

Choice of typefaces should be tailored for the project's needs. If doing work primarily for the Web, the typefaces should be optimised and license-able for Web use. We have accounts at several subscription services, so we recommend you browse and consider those first when choosing typefaces for a project. You'll find a list of these services, along with other relevant links, below.

### Presentation and Mock-ups

You're free to use whatever tools you prefer to present proposals and/or mock-ups to clients, but we recommend using InVision for prototyping UI work. It's a robust service, has an almost seamless integration with Sketch and has proved to be client-friendly. For other types of work, we're partial to using short and to-the-point PDF presentations.

### Design Files

As soon as the design work has been approved and it's moving to code, you should upload all of the original design files plus any necessary assets (e.g. fonts, icons, original images, etc) to Google Drive, under the client's folder (go to Drive > Clients and check for a folder with the client's name, or create one in case it doesn't exist). **This is required procedure, even for projects you're the only one working on**. Whenever there have been major updates to any of the design files or assets, you should re-upload them (Drive takes care of file history and versioning).

### Resources

#### Tools

* https://www.invisionapp.com
* https://rightfontapp.com

#### Typeface Services

* https://typekit.com/
* https://www.typography.com/cloud/welcome
* https://www.myfonts.com/info/mls
* https://fontstand.com

#### Suggestions

* Whenever possible, try to anticipate your colleagues' involvement in the project beforehand, and plan your software use accordingly.

**[🚡 back to top](#table-of-contents)**

## HTML

### Semantics

We should make an effort to produce valid semantic HTML, that takes advantage of the full potential of HTML5's tags to produce clean code, that is readable by humans and machines alike.

### Templating Languages

We use different templating engines, depending on the project's backend.

* **ERB** for Rails/Middleman
* **Twig** for Symfony
* **Handlebars** for Ember
* **JSX** for React

### Resources

#### Learn More

* https://codepen.io/mi-lee/post/an-overview-of-html5-semantics

**[🚡 back to top](#table-of-contents)**

## CSS

### Preprocessor

We use SASS as the standard for our styling needs.

### Methodology

We use [Airbnb's css style guide](https://github.com/airbnb/css) as the basis for our CSS methodology, with some minor exceptions and adaptations:

#### Naming

Our naming conventions follow BEM's methodology, with a couple of twists:

* Nested single class names should start with a dash:
    * 👌 `.header .-button` is good
    * ❌ `.header .button` should be avoided

* Classes shouldn't be chained above three levels:
    * 👌 `.header__navigation .-link` is good
    * ✅ `.header__navigation__links .-link` is ok
    * ❌ `.header__navigation__links__link` should be avoided

* Classes shouldn't nest more than three levels deep:
    * 👌 `.header .-button .-link` is good
    * ❌ `.header .-button .-link .-title` should be avoided

* We should try to follow the “widget wrapper” pattern. Widgets are unique and follow the BEM naming conventions, while subclasses are global and follow simple semantic conventions:
    * `.header .header__navigation .-link`
    * `.block .block__links .-link`

* For performance reasons, we should try to nest classes as little as possible.
    * 👌 `.block ul a` is good
    * ❌ `.block ul li a` should be avoided

* We prefer the use of underscores and dashes over Airbnb's suggested camelCase approach;

#### Units

**Usage of EMs/REMs for everything is recommended**. REMs should be the default, EMs should be used when we need local dependencies and PXs only for the rare cases when things aren't supposed to scale at all. EMs and REMs should be calculated through an helper, and never input manually (if you're setting up a design system, the boilerplate comes with a simple functions called [ds-em](https://github.com/kollegorna/design-system-boilerplate/blob/master/scss/config/_type.scss#L55) and [ds-rem](https://github.com/kollegorna/design-system-boilerplate/blob/master/scss/config/_type.scss#L59)).

### Responsive Breakpoints

Regardless of how pages have been designed, breakpoints should be structured according to a mobile-first mindset. This means all default styles should be targeted at the mobile version, and overrides progressively introduced for larger screens, through the use of media queries. Using mobile-specific breakpoints is ok when trying to override default values specifically and uniquely for mobile.

```css
.element {
  /* Default styles */

  @include media(tablet) {
    /* Tablet overrides */
  }

  @include media(desktop) {
    /* Desktop overrides */
  }
}
```

### File Structure

Different projects may require different file structuring, but in general it's a good idea to split styles into several files and, for larger projects, organise them into folders. Please use your best judgement here, and choose the structure that you believe suits the project best. If your css files are getting too long, it's probably a good sign that you need to reconsider how files are structured.

### Design Systems

It's common for us to work on several different projects for the same client. When this happens, we've found it useful to develop a collection of global, reusable styles — which we call a Design System. When declared as a dependency on a project, a design system gives us a nice collection of sensible defaults we can use to get started faster. Should the need arise, it also lets us update one default style across all projects related to a specific client. If you believe a design system would be beneficial in the long run, there is [a boilerplate with some sensible defaults in place](https://github.com/kollegorna/design-system-boilerplate). The repo includes instructions on how to set it up, as well as recommendations on how to seamlessly include it in your project without disrupting your workflow.

### Resources

#### Learn More

* https://github.com/airbnb/css#oocss-and-bem
* http://getbem.com/introduction/
* http://www.intelligiblebabble.com/a-pattern-for-writing-css-to-scale

#### Suggestions

* Always use spaces instead of tabs to indent CSS (make sure [.editorconfig](#add-editorconfig) is properly set up and you can safely forget about it);

**[🚡 back to top](#table-of-contents)**

## JavaScript

### Style

We use [Airbnb's JS style guide](https://github.com/airbnb/javascript) as reference.

### ES6

We use ES6 together with [Babel](https://babeljs.io/), to ensure the code is compiled down into browser-compatible javascript.

### jQuery

While jQuery is a great library for querying and manipulating the DOM, it is sometimes easy to over-rely on it. It's ok to use it for larger projects, where a lot of jQuery's functionality is required, or when building quick prototypes, but we should refrain from using it whenever it's clear that ES6 would allow us to build the exact same functionality with little code. If you're only using jQuery as a selector, consider using [Sizzle](https://github.com/jquery/sizzle) instead.

Our suggestion is not to rely on jQuery for animations or transitions, if the same effects can be accomplished purely with CSS (using javascript for class toggling only).

### Routing

On simple static sites, we encourage the use of [DOM routing](https://www.paulirish.com/2009/markup-based-unobtrusive-comprehensive-dom-ready-execution/), in order to keep the code scoped, cleaner and more readable.

### Resources

#### Learn More

* https://es6.io/

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

```svg
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

Animating SVGs through CSS or SMIL should be approached with caution. Browser support for CSS Animations is buggy and not wide enough yet, and SMIL is not supported in IE/Edge and will soon be deprecated everywhere else. For the time being, we suggest you use JavaScript libraries such as [Snap.svg](http://snapsvg.io/) or [Velocity.js](http://velocityjs.org/).

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

* http://nukesaq88.github.io/Pngyu/
* https://imageoptim.com (https://imageoptim.com/)
* https://tinypng.com/ (for both PNG and JPG)

#### Tutorials

* https://css-tricks.com/accessible-svgs

**[🚡 back to top](#table-of-contents)**

## Structured Data

Use of JSON-LD and Schema.org (http://schema.org/) definitions to markup content is strongly encouraged whenever its benefits for the project are self-evident. You should use your best judgement in assessing whether or not the benefits justify the time invested in properly structuring data.

### Resources

#### Learn More

* https://developers.google.com/search/docs/guides/intro-structured-data

**[🚡 back to top](#table-of-contents)**

## Libraries

We have a liberal but cautious stance on libraries. We believe they can greatly increase the efficiency of our developers, but also make projects harder to maintain, so they should be used consciously. You're an expert and will have to make the best decision for each and every project.

### Suggested Libraries

This is a small list of libraries we have used and tested exhaustively, and encourage you to use:

#### [Foundation for Sites](http://foundation.zurb.com/sites.html)

Great framework for rapidly building prototypes and good enough to be used in production. Please bear in mind that just because we use Foundation, we don't have to opt for its components for everything in that project. If you think there is a better library for handling tabs, for example, then go ahead and use that. For most projects, we recommend you use SASS mixins instead of inline class names.

### Resources

#### Suggestions

* When using libraries in production, try to only require/import the pieces you need — e.g. if you're using Foundation's grid, only add Foundation's core, its grid component and any dependencies it may have;
* When adding a library as a dependency, you should specify a version, to prevent builds from breaking with future updates;

**[🚡 back to top](#table-of-contents)**

## Accessibility

Building applications and websites that are usable for as many people (and bots) as possible is in our backbone. It's not some extra topping on the ice cream but rather something that should be considered during the entire process of a project.

### WCAG 2.0 Level AA

Code should be compliant with WCAG 2.0 Level AA. This is something that needs to be considered in the design process as well as one of the ingredients in the WCAG mix is color contrast.

### Best Practices

#### Interactive Elements

When working with elements users are supposed to click or tap on, use HTML tags meant for that purpose:

```javascript
$('.btn').on('click', ...
```

✅ DO:

```html
<button class="btn" type="button">Go baby!</button>
```

❌ DON'T:

```html
<div class="btn">Go baby!</div>
```

### Resources

#### Learn More

* http://webaim.org/intro/ (required reading)
* https://madebysidecar.com/journal/accessibility-basics-for-designers
* http://webaim.org/standards/wcag/checklist
* https://developer.mozilla.org/en-US/docs/Web/Accessibility
* https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA
* https://webbriktlinjer.se/r/1-utga-fran-wcag-2-0-niva-aa/ (Swedish)

#### Docs & specs

* http://checkers.eiii.eu/en/pagecheck2.0/
* http://achecker.ca/checker/
* http://cynthiasays.com/
* http://www.tawdis.net/ingles.html?lang=en

#### Tools

* http://webaim.org/resources/contrastchecker/

#### Suggestions

* Whenever possible, choose frameworks and libraries that are accessible by default. Foundation for example has accessibility in its backbone (http://foundation.zurb.com/sites/docs/accessibility.html).
* Make sure ARIA tags are used appropriately. Consider using JavaScript to add them (https://codepen.io/dencarlsson/pen/vmaqNQ).

**[🚡 back to top](#table-of-contents)**

## Performance

### Best Practices

#### Embedding external resources

We should avoid placing external resource embed codes in <head> that work in a synchronous manner and therefore blocks rendering of the page. The more such an occasions, the later users start seeing the page. Asynchronous CSS is encouraged, but due to its complex nature it is okay to have a (single preferably) synchronous CSS file reference. JavaScript embeds should be placed right before </body> tag preferably with [`defer` or `async`](http://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html) attributes inserted along.

```html
<!DOCTYPE html>
<html>
  <head>
    <link rel="stylesheet" href="/main.css">
  </head>
  <body>
    ...
    <script src="/main.js" defer></script>
  </body>
</html>
```

#### Inlining JavaScript

In cases (e.g. Modernizr) when there's a need to run some JavaScript code in `<head></head>` we should keep it as short as possible and inline it by making sure it's minified in the production.

```html
<head>
  ...
  <script>
    ;!function(e,t,n){function o(e,t)...
  </script>
</head>
```

Even though ideally all the JavaScript code should be placed in external files, it's sometimes okay to inline it at the end of the document, e.g.:

```html
    ...
  <script src="/main.js" defer></script>
  <script>
    // inlined JS
  </script>
</body>
</html>
```

Avoid inlining JavaScript everywhere else.

#### Inlining CSS

CSS should only be inlined in `<head></head>`.

#### Performant jQuery code

jQuery selectors are expensive therefore we should cache jQuery selectors into variables.

✅ DO:

```javascript
//
var $win = $(window);
$win.on('scroll', ...
$win.on('resize', ...

$('.items', function() {
  var $item = $(this);
  $item.attr(...
  $item.on(...
});
```

❌ DON'T:

```javascript
$(window).on('scroll', ...
$(window).on('resize', ...

$('.items', function() {
  $(this).attr(...
  $(this).on(...
});
```

Also be sure to check [jQuery code recommendations](https://learn.jquery.com/performance/).

#### Performant CSS code

Avoid selectors with more than three levels:

* 👌 `.box` is great
* ✅ `.box .author .name` is good
* ❌ `.box .author .name .first` is bad

### Resources

#### Learn more

* https://learn.jquery.com/performance/

#### Tools

* https://developers.google.com/speed/pagespeed/
* https://tools.pingdom.com (only on live sites since the last checked sites are displayed on the page… don't spill any secrets)

**[🚡 back to top](#table-of-contents)**

## Support and Compatibility

While we are committed to ensuring the things we build can be used by as many users as possible, we also believe deep retroactive support hinders progress, so we try to make sure our work strikes the right balance of browser and device support.

### Support Checklist

For most projects, we should test exhaustively and make sure things work as expected in the following:

#### Mobile Platforms
*Latest versions of…*

* iOS
* Android

#### Mobile browsers
*Latest versions of…*

* Mobile Safari
* Chrome for Mobile

#### Desktop Platforms
* Windows
* MacOS
* Linux

#### Desktop browsers
*Two latest versions of…*

* Chrome
* Firefox
* Safari

*And it should also work in…*

* Microsoft Edge
* Internet Explorer 10 and up

This doesn't mean that it has to look exactly the same across different browsers. But rather that the overall functionality should work and that the content should be accessible.

### Resources

#### Suggestions

* When doing testing (and primarily design testing) we should do it both on retina and non retina screens. For example some fonts might have an excellent readability on smaller sizes on retina screens but are almost unreadable without retina.

**[🚡 back to top](#table-of-contents)**

## Tools

### Task Runners

We regularly use Gulp, but are open to trying other alternatives.

### Dependency Managers

We have historically used Bower for most of our frontend dependencies, but we're slowly retiring it. Whenever possible, Yarn should be used instead.

On WordPress projects, we also use Composer.

### Linters

#### Javascript

We recommend using [ES lint](http://eslint.org/) with [Airbnb's config](https://www.npmjs.com/package/eslint-config-airbnb).

#### SASS/CSS

We recommend using either [stylelint](https://stylelint.io/) or [sass-lint](https://github.com/sasstools/sass-lint).

**[🚡 back to top](#table-of-contents)**

## Checklists

### Setup Checklist

- [ ] Setup yarn's package.json
- [ ] Add [autoprefixer](https://github.com/postcss/autoprefixer)
- [ ] Add [.editorconfig](http://editorconfig.org/)
- [ ] Add [babeljs](https://babeljs.io/) if using ES2015
- [ ] Set up [DOM-based routing](https://www.paulirish.com/2009/markup-based-unobtrusive-comprehensive-dom-ready-execution/) if working on a static site
- [ ] Add [inline_svg](https://github.com/jamesmartin/inline_svg) or [SVGInjector](https://github.com/iconic/SVGInjector)

### Pre-Shipping Checklist

- [ ] Optimise all raster images
- [ ] Test in all major browsers
- [ ] Move design system outside main repo, if applicable

**[🚡 back to top](#table-of-contents)**
