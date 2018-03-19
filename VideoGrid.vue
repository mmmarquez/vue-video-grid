<template>
  <section class="_video-grid">
    <div class="_video-grid-container">
        <div class="_video-item-wrap" v-for="(embed, index) in embedMedia" :key="index" v-if="embedMedia" v-html="embed"></div>
    </div>
  </section>
</template>
<script>
function debounce(func, wait, immediate) {
  var timeout;
  return function() {
    var context = this,
      args = arguments;
    var later = function() {
      timeout = null;
      if (!immediate) func.apply(context, args);
    };
    var callNow = immediate && !timeout;
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
    if (callNow) func.apply(context, args);
  };
}

export default {
  name: "video-grid",
  props: {
    embedMedia: Array,
    itemPercentage: Number
  },
  methods: {
    aspectRatio(h, w) {
      return h / w;
    },
    percentageRatio(percent, dimension) {
      return percent * dimension / 100;
    },
    videoGrid(videoContainer, videoWrap, allVideos) {
      allVideos.forEach(video => {
        let videoDimensions = {
          width: video.clientWidth,
          height: video.clientHeight
        };
        video.removeAttribute("height");
        video.removeAttribute("width");
        let videoParent = video.parentNode;
        let parentDimensions = {
          width: videoParent.offsetWidth,
          height: videoParent.offsetWidth
        };
        video.setAttribute(
          "width",
          this.percentageRatio(this.itemPercentage, parentDimensions.width)
        );
        video.setAttribute(
          "height",
          this.percentageRatio(this.itemPercentage, parentDimensions.width) *
            this.aspectRatio(videoDimensions.height, videoDimensions.width)
        );
      });
    }
  },
  computed: {
    screenWidth() {
      return {
        width:
          window.innerWidth ||
          document.documentElement.clientWidth ||
          document.body.clientWidth,
        height:
          window.innerHeight ||
          document.documentElement.clientHeight ||
          document.body.clientHeight
      };
    }
  },
  mounted() {
    let videoContainer = document.querySelectorAll("._video-grid-container");
    let videoWrap = document.querySelectorAll("._video-item-wrap");
    let allVideos = document.querySelectorAll("._video-item-wrap iframe");
    this.videoGrid(videoContainer, videoWrap, allVideos);

    var myEfficientFn = debounce(() => {
      this.videoGrid(videoContainer, videoWrap, allVideos);
    }, 250);

    window.addEventListener("resize", myEfficientFn);
  },
  created() {}
};
</script>
