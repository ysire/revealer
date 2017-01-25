# revealer

[![Greenkeeper badge](https://badges.greenkeeper.io/cdaringe/revealer.svg)](https://greenkeeper.io/)

![](https://img.shields.io/badge/standardjs-%E2%9C%93-brightgreen.svg)

build pure reveal.js presentations without cruft _or_ cognitive overload.

## install

`npm i --save reveal.js revealer`

# usage

- add reveal.js content

```html
<!-- src/index.html -->
<!doctype html>
<html>
	<head>...</head>
	<body>
		<div class="reveal">
			<div class="slides">
				<section>Slide 1</section>
			</div>
		</div>
    <script src="reveal.js"></script>
		<script>
			Reveal.initialize({ ... });
		</script>
	</body>
</html>
```

- to quickstart, use the reveal.js default!
  - `mkdir src/ && cp node_modules/reveal.js/index.html ./src/`

- next consider adding some helpful npm scripts:

```json
{
  "scripts": {
    "serve": "revealer --serve",
    "build": "revealer --build [dest-dir]"
  }
}
```

- modify your content in `src`, watch it livereload, and still get all of reveal.js' rails included for free.

**finally**, you're left with a simple, beautiful folder structure.  no cruft.  just your package.json and your presentation content!

```
$ tree -L 1
.
├── node_modules
├── package.json
├── public // output from revealer --build
└── src
```

:game_die::game_die::game_die::game_die::game_die: yahtzee!

let me use `revealer` programmatically you say.  no problem.

```js
'use strict'

const rvlr = require('revealer')
const path = require('path')

// copy reveal.js content (from node_modules) into ./staging
// copy your presentation `./src` into `./staging`
rvlr.stage.all()

// run the reveal.js content server.  livereloads against  watched source content
rvlr.serve()

// build! static assets can be served from `build/` dir
rvlr.build() // optional arg for dir to build into

```

<big>**official API docs live [here](https://cdaringe.github.io/revealer/)**</big>

# why

starting a reveal.js project should be as simple as `npm install reveal.js`, authoring some content, and hitting the go-button.  unfortunately, there's more to it.

reveal.js is amazing.  however, when it's time to actually build a presentation, your choices to quickly scaffold a presentation are both less-than-ideal.  your two primary options to start building a presentation are either:

1. clone and hack the reveal.js source, or
1. use the npm package

let's discuss these strategies, then discuss `revealer`'s solution to the cons on both sides.

### the ol' clone-n-hack

**option #1**.  the reveal.js source code comes with a great set of rails.  it's packed with valuable css, plugins, and tooling to get your developing your presentation quickly (e.g. file server & live reload).  however, cloning a repo then modifying it isn't really a great way to use a _library_.  instead of consuming it as a library, you end up using it as a standalone application.  what are this strategy's weaknesses?

1. what happens when you need upstream updates? merge conflicts are almost guaranteed, at some point.
1. how does your presentation repository structure look? it looks like the reveal.js source code, not simply your precious presentation content.  this yields some degree complication & misdirection.

many reveal.js projects choose this path.

### consume the npm package

**option #2**. `npm install reveal.js` _is a thing_.  you can install it as a dependency.  however, when done so, you no longer have rails.  the helpful reveal.js assets are still nested inside of `node_modules/reveal.js/...`, but you don't have a runable solution ready to go.  that is to say, the ease of developing a presentation suddenly becomes much more difficult.  you need to manually construct your page, your assets, and development workflow, vs. using those which reveal.js already provides for you. these bells and whistles don't need to be redesigned--reveal.js' workflow is just fine!  some projects, like [reveal-boilerplate](https://github.com/Retozi/reveal-boilerplate), do _rewire_ up a set of assets into a full presentation, BUT, is it worth reimplementing all of the reveal.js tooling again in your own project?  probably not!

### give me the best of both worlds, please!

here's what we want:
  - quick setup time for a reveal.js project
  - to use reveal.js as a dependency, and permit rolling version updates
  - to keep our project source looking like _just_ our source, not the reveal.js source
  - to maintain ease of use, keeping the reveal.js rails intact.  that is, make  plugins/css/tooling that ship with reveal.js accessible

**revealer achieves these goals**. specifically, it:

  - installs `reveal.js`'s dev dependencies within the reveal.js dependency
  - watches your `src` dir, and copies them into the `reveal.js` package src
  - runs `reveal.js`'s server-watcher-browser-launcher tooling.
  - onbuild, `revealer --build`, automatically merges your content with reveal.js' and outputs a runnable, static website!

yahoo!
