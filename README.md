# vue-plyr
>v7.0.0 - [Changelog](https://github.com/redxtech/vue-plyr/blob/master/changelog.md)

A vue component for the plyr video & audio player.

This is useful for when you want a nice video player in your Vue app.

It uses [plyr](https://plyr.io) by [sampotts](https://github.com/sampotts) for the players.

Supported player types: HTML5 video, HTML5 audio, YouTube (div & progressive
enhancement), and Vimeo (div & progressive enhancement).

## Installation
```bash
yarn add vue-plyr # or npm i vue-plyr
```

### Module
```js
// In your main vue file - the one where you create the initial vue instance.
import Vue from 'vue'
import VuePlyr from 'vue-plyr'
import 'vue-plyr/dist/vue-plyr.css'

// The second argument is optional and sets the default config values for every player.
Vue.use(VuePlyr, {
  plyr: {
    fullscreen: { enabled: false }
  }
})
```

### SSR [(more below)](#ssr)
For SSR you can import the SSR optimized module, found at `./dist/vue-plyr.ssr.js`.
There is a more in depth description on how to use it with [nuxt](#nuxt) below.

### Browser
In the browser you can include it as you would any other package with unpkg, along
with the stylesheet.

```html
<script type="text/javascript" src="https://unpkg.com/vue"></script>
<script type="text/javascript" src="https://unpkg.com/vue-plyr"></script>
<link rel="stylesheet" href="https://unpkg.com/vue-plyr/dist/vue-plyr.css" />

<!-- You will also need to install the component during app creation -->
<script>
  window.Vue.createApp({}).mount('#app')
</script>
```

## Usage
Once installed, it can be used in a template as simply as:
```vue
<!-- video element -->
<vue-plyr>
  <video poster="poster.png" src="video.mp4">
    <source src="video-720p.mp4" type="video/mp4" size="720">
    <source src="video-1080p.mp4" type="video/mp4" size="1080">
    <track kind="captions" label="English" srclang="en" src="captions-en.vtt" default>
  </video>
</vue-plyr>

<!-- audio element -->
<vue-plyr>
  <audio>
    <source src="audio.mp3" type="audio/mp3"/>
    <source src="audio.ogg" type="audio/ogg"/>
  </audio>
</vue-plyr>

<!-- youtube iframe with progressive enhancement (extra queries after the url to optimize the embed) -->
<vue-plyr>
  <div class="plyr__video-embed">
    <iframe
      src="https://www.youtube.com/embed/bTqVqk7FSmY?iv_load_policy=3&modestbranding=1&playsinline=1&showinfo=0&rel=0&enablejsapi=1"
      allowfullscreen allowtransparency allow="autoplay">
    </iframe>
  </div>
</vue-plyr>

<!-- youtube div element -->
<vue-plyr>
  <div data-plyr-provider="youtube" data-plyr-embed-id="bTqVqk7FSmY"></div>
</vue-plyr>

<!-- vimeo iframe with progressive enhancement (extra queries after the url to optimize the embed) -->
<vue-plyr>
    <div class="plyr__video-embed">
      <iframe
        src="https://player.vimeo.com/video/76979871?loop=false&byline=false&portrait=false&title=false&speed=true&transparent=0&gesture=media"
        allowfullscreen allowtransparency allow="autoplay">
      </iframe>
    </div>
  </vue-plyr>

<!-- vimeo div element -->
<vue-plyr>
  <div data-plyr-provider="vimeo" data-plyr-embed-id="76979871"></div>
</vue-plyr>
```

## Player Instance
To access the player instance, you can use the `player` property from the `refs` attribute.

```html
<template>
  <vue-plyr ref="plyr"></vue-plyr>
</template>

<script>
export default {
  name: 'Component',
  mounted () {
    console.log(this.$refs.plyr.player)
  }
}
</script>
```

## Events
If you want to capture events from the plyr instance, you can do so by
accessing the player instance through the `ref` attribute and using
that object for events, as you would with a vanilla plyr instance.

Valid events are [here](https://github.com/sampotts/plyr#events).

```html
<template>
  <vue-plyr ref="plyr"></vue-plyr>
</template>
<script>
export default {
  name: 'Component',
  mounted () {
    this.$refs.plyr.player.on('event', () => console.log('event fired'))
  }
</script>
```

## Options
For custom options you can pass an `options` prop which is an object
that will be passed to the `new Plyr()` creation. Available options
[here](https://github.com/sampotts/plyr#options). I have added a new
option (`hideYouTubeDOMError`) that hides the error that is always
logged when destroying a YouTube player. It defaults to `true`, and you
can disable it and see the error by setting it to false.

You can also specify the default options when registering the plugin
(these will be ignored if you specify a player-specific options object via props):

```js
Vue.use(VuePlyr, {
  plyr: {
    fullscreen: { enabled: false }
  }
})
```

## SSR
### Nuxt
This should support SSR out of the box. For [nuxt](https://nuxtjs.org/), create a file called `vue-plyr.js` in your plugins folder containing
only these three statements:
```js
import Vue from 'vue'
import VuePlyr from 'vue-plyr/dist/vue-plyr.ssr.js'

// The second argument is optional and sets the default config values for every player.
Vue.use(VuePlyr, {
  plyr: {
    fullscreen: { enabled: false }
  }
})
```
Then, in your `nuxt.config.js` file add `'~/plugins/vue-plyr'` to the plugins array. The `vue-plyr` element should be globally registered now.

You will also want to add `vue-plyr/dist/vue-plyr.css` to your css array in the same file.

The `nuxt.config.js` file should at minimum include this:
```js
export default {
  plugins: [
    '~/plugins/vue-plyr'
  ],
  css: [
    'vue-plyr/dist/vue-plyr.css'
  ]
}
```

## Author
**vue-plyr** © [RedXTech](https://github.com/redxtech), Released under the [MIT](./LICENSE.md) License.
