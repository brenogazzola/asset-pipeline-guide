## Intro

Rails 7 brought with it an overhaul of the Asset Pipeline in the form of multiple new gems that either introduced a new way of handling assets (importmaps-rails), or broke down existing gems (propshaft, jsbundling-rails, cssbundling-rails) into smaller parts that make use of existing standalone tools and therefore support modern features that were previously unavailable.

While a few of these gems are still in pre-release status, their maintainers, contributors and early adopters have already deployed them to production. However, widespread adoption is currently being hampered by the lack of detailed documentation and some missing features that would allow them to support more use cases.

With the creation of the new Rails Discord channel for people interested in contributing to Rails, I think it's a good time to coordinate with the community so we can pare down the rough edges. Therefore I'd like to start this thread so we can make a more visible demonstration that there are people working to improve things and provide central source of knowledge while all the related guides haven't been updated.

## Gems: explanations and recommendations

I'm writing this not as maintainer of any of these gems, but as a regular contributor and someone who has deployed most of them in the monolithic production app that my own company relies on. Therefore corrections and suggestions are welcome (including grammar. English is hard).

**This is very much a work in progress**.

### Sprockets

- Status: Maintained

- Notes: Several fixes and updates are only available in the edge version.

The original asset pipeline gem, Sprockets provides bundling and digesting for javascript, css and image files. Built before node was widely adopted, Sprocket's "everything included" approch worked well for many years. However, as frontend programming became more complex, it started to fall behind dedicated tools like webpack. It was the only officially supported gem up to Rails 5.2, when Webpacker was introduced, and remained the recommended approach until Rails 7, when it was replaced by Import Maps.

**If you have an existing app with Sprockets**: You can continue using Sprockets. It is no longer under active development, but still receives updates to ensure compatibility with the newer parts of the asset pipeline.

**If you are starting a new app**: Only use Sprockets if you absolutely do not want to deal with node/yarn and are not ready for an import maps approach. Or if you'd rather not rely on Propshaft, due to its pre-release status.

**If you are just getting started on Rails**: Don't use it. Your time will be better spent learning one of the more modern gems below. Check their individual recommendations to make a decision.

### Webpacker

- Status: Retired

- Notes: All work done for the unreleased version 6 went to Shakapacker.

Rails has always had a complicated relationship with vanilla javascript, and to get around perceived limitations of the language, it adopted coffeescript as the default. Things changed with the arrival of ES6 and Babel for transpilation, but getting everything working so that you could use modern features of the language while remaining compatible with older browsers was difficult. Webpacker was created to solve that problem and for some time provided the bridge for developers interested in investing more heavily in the javascript ecosystem.

Five years later, all browsers oficially support ES6, so transpilation was no longer necessary and the extra complexity not worth for many developers. This lead to Webpacker being retired.

**If you have an existing app with Webpacker**: Give some serious consideration to replacing it. If your app is a SPA, specially a React one, then Shakapacker is your friend, as it's the official sucessor to Webpacker. If you are mostly using Hotwired and doing server side rendering, jsbundling-rails + webpack is an easy migration (and you can later migrate to the excellent esbuild).

**If you are starting a new app**: Don't use it. SPAs should go with Shakapacker, the official sucessor of Webpacker, which supports things like HMR, and Hotwired/Tailwind based apps should go with importmaps-rails or jsbundling-rails.

**If you are just getting started on Rails**: Check the recommendation above.

### Import maps

- Status: Released, in active development

- Notes: A shim is available to provide support for older browsers

Rails 7 apps defaults to the importmaps-rails gem, which shares the same name as the import maps feature in browsers. This gem relies on the trifecta of HTTP2 (which eliminates the penalty of many small files), wide spread support for ES6 and the import map feature (which can the added to legacy browsers and Safari through a shim).

The main advantage of this gem is that it eliminates the need for node/yarn and the complex javascript tooling that modern frontend development relies on.

**If you have an existing app**: This is a very different approach to handling javascript and css files. You will be better served if you stick to the node ecosystem and go with Shakapacker or jsbundling-rails/cssbundling-rails/propshaft.

**If you are starting a new app**: If you are building an SPA, this is not your friend. However if your app will be doing server side rendering and using Hotwired and tailwind, importmaps-rails is the preferred approach.

**If you are just getting started on Rails**: Check the recommendation above.

### Propshaft

- Status: Alpha, in active development

- Notes: Already deployed to production in Basecamp (and by me, FWIW).

The sucessor to Sprockets, it has a much smaller scope than its predecessor and relies on the companion gems jsbundling and cssbundling. Propshaft's only job is to digest files, fix references so that they use the digested filename, and move everything to the public folder. While jsbundling-rails and cssbundling-rails are only wrappers around other tools, propshaft does all the work itself and it is still a small gem. Therefore it is an excellent place to start contributing and you can read its entire code base in less than an hour.

**If you have an existing app**: If you are currently on Webpacker AND feel comfortable using alpha version libraries AND reading through source code, sure! I'm using it in production and it works great. Otherwise, stay where you are or take a look at importmaps-rails and shakapacker.

**If you are starting a new app**: Same as above.

**If you are just getting started on Rails**: Don't. It has almost no documentation, very few users, and its still in alpha. You are already going to be spending a lot of effort learning other things, so you'd be better of learning importmaps-rails (hotwired/tailwind) or shakapacker (SPAs like React)

### jsbundling-rails

- Status: Released, in active development

- Notes: -

Provides javascript bundling features that were previously handled by Sprockets and Webpacker. Supports webpack, esbuild and rollup.

**If you have an existing app**:

**If you are starting a new app**:

**If you are just getting started on Rails**:

### cssbundling-rails

- Status: Released, in active development

- Notes:

Provides css bundling features that were previously handled by Sprockets and Webpacker. Supports Tailwind, Bootstrap, Bulma, PostCSS and Dart Sass.

**If you have an existing app**:

**If you are starting a new app**:

**If you are just getting started on Rails**:

### dartsass-rails

- Status: Released, in active development

- Notes:

Same as cssbundling-rails, but specific for dart. A wrapper around the platform specific versions of dart, instead of the yarn versions.

**If you have an existing app**:

**If you are starting a new app**:

**If you are just getting started on Rails**:

### Simple recommendations of which gems to use in your app:

- Your app is a SPA: Shakapacker

- You want to use Hotwired and Tailwind: Import maps

- Everyone else: Propshaft, jsbundling (esbuild) and dartsass-rails.

### If you'd like to contribute

Open a PR for this guide at [Github](https://github.com/brenogazzola/asset-pipeline-guide).

### Github Pages

- [Sprockets](https://github.com/rails/sprockets)

- [Propshaft](https://github.com/rails/propshaft/)

- [Shakapacker](https://github.com/shakacode/shakapacker)

- [importmap-rails](https://github.com/rails/importmap-rails)

- [jsbundling-rails](https://github.com/rails/jsbundling-rails)

- [cssbundling-rails](https://github.com/rails/cssbundling-rails)

- [dartsass-rails](https://github.com/rails/dartsass-rails)

- [tailwind-rails](https://github.com/rails/tailwindcss-rails)

### Upgrade Guides

- [From Turbolinks to Hotwired:Turbo](https://github.com/hotwired/turbo-rails/blob/main/UPGRADING.md)

- [From Sprockets/Wepacker to Propshaft](https://github.com/rails/propshaft/blob/main/UPGRADING.md)

### Relevant Information

- [Modern web apps without JavaScript bundling or transpiling](https://world.hey.com/dhh/modern-web-apps-without-javascript-bundling-or-transpiling-a20f2755)

- [Introducing Propshaft](https://world.hey.com/dhh/introducing-propshaft-ee60f4f6)

### Contributors to this guide

### Changelog

- 2022-06-17: First version
