# vue-video-grid

A Vue.js component for displaying an iframe video grid while keeping the original aspect ratio

Currently under development.

## Installation

```js
npm i --save vue-video-grid
```

## Usage

Import

```js
import VideoGrid from "vue-video-grid";
```

Register

```js
export default {
  components: {
    VideoGrid
  }
};
```

HTML

```html
<template>
    <VideoGrid :embedMedia="['<iframe>...</iframe>', '<iframe>...</iframe>']" :itemPercentage="96" />
</template>
```

Style

Add your own styles.

```scss
._video-grid {
  ._video-grid-container {
    display: flex;
    flex-direction: row;
    flex-flow: wrap;
    align-items: center;
    justify-content: center;
    width: 100%;
    ._video-item-wrap {
      margin-bottom: 1%;
      width: 33.333%;
      iframe {
        border: 3px solid black;
      }
    }
  }
}
```

## Props

`:embedMedia="[ARRAY]"`

Array containing the `<iframes></iframes>` to display.

`:itemPercentage="NUMBER"`

Percentage number for total width of video in container. This is used to add padding between grid items.
