<template>
  <video ref="videoPlayerRef" class="video-js"></video>
</template>

<script>
import Videojs from "video.js";
import "video.js/dist/video-js.css";

export default {
  props: {
    src: {
      type: String,
      default: "https://test-streams.mux.dev/x36xhzz/x36xhzz.m3u8", //可以用这个流来测试
    },
  },
  data() {
    return {
      player: null,
      videoPlayerOption: {
        controls: true, //确定播放器是否具有用户可以与之交互的控件。没有控件，启动视频播放的唯一方法是使用autoplay属性或通过Player API。
        poster: "", //封面
        autoplay: true, //自动播放属性,
        muted: true, // 静音播放
        preload: "auto", //建议浏览器是否应在<video>加载元素后立即开始下载视频数据。
        fluid: true,
      },
    };
  },
  mounted() {
    console.log(this.src, "this.src");
    this.player = Videojs(
      this.$refs.videoPlayerRef,
      this.videoPlayerOption,
      () => {
        this.player.src({ src: this.src, type: "application/x-mpegURL" });
        this.player.play();
      }
    );
  },
  beforeDestroy() {
    // 一定要销毁
    if (this.player) {
      this.player.dispose();
    }
  },
};
</script>