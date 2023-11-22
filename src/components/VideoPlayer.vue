<template>
  <div>
    <video ref="videoPlayer" class="video-js vjs-defaultskin"></video>
  </div>
</template>
<script>
import "video.js/dist/video-js.css";
import videojs from "video.js";
export default {
  name: "VideoJsPlayer",
  props: {
    videoUrl: String,
  },
  data() {
    return {
      player: null,
      videoOptions: {
        autoplay: true,
        controls: true,
        preload: true,
        fluid: true,
        width: "640px",
        height: "267px",
        sources: [
          {
            src: this.videoUrl,
            type: "video/mp4",
          },
        ],
      },
    };
  },

  // Initializing the video player upon component mounting for immediate interaction.
  mounted() {
    videojs(this.$refs.videoPlayer, this.videoOptions);
  },
  // Destroying the video player upon component destruction for cleanup.
  beforeDestroy() {
    if (this.player) {
      this.player.dispose();
    }
  },
};
</script>
