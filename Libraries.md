## Libraries

We have a liberal but cautious stance on libraries. We believe they can greatly increase the efficiency of our developers, but also make projects harder to maintain, so they should be used consciously. You're an expert and will have to make the best decision for each and every project.

### Suggested Libraries

This is a small list of libraries we have used and tested exhaustively, and encourage you to use:

#### [Foundation for Sites](http://foundation.zurb.com/sites.html)

Great framework for rapidly building prototypes and good enough to be used in production. Please bear in mind that just because we use Foundation, we don't have to opt for its components for everything in that project. If you think there is a better library for handling tabs, for example, then go ahead and use that. For most projects, we recommend you use SASS mixins instead of inline class names.

#### [Micromodal.js](https://micromodal.now.sh)

Micromodal.js is a lightweight, configurable and a11y-enabled modal library written in pure JavaScript.

#### Other libraries

For other vanilla JS plugins, check [PlainJS](https://plainjs.com/javascript/plugins).

### Resources

#### Suggestions

* When using libraries in production, try to only require/import the pieces you need — e.g. if you're using Foundation's grid, only add Foundation's core, its grid component and any dependencies it may have;
* When adding a library as a dependency, you should specify a version, to prevent builds from breaking with future updates;

**[👈 back to README.md](README.md)**