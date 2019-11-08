<template>
  <div class="audio-comp">
    <div class="audio-bar" @click="play">
      <div class="time">{{duration}}"</div>
      <div v-if="isPlay" class="play-ani" :style="{backgroundImage: 'url('+ audio_ani +')'}"></div>
      <div v-else class="play-ani" :style="{backgroundImage: 'url('+ audio_stop +')'}"></div>
    </div>
  </div>
</template>

<script>
import audio_stop from "./audio_stop.png";
import audio_ani from "./audio_ani.gif";
export default {
  props: ["audioSrc"],
  data() {
    return {
      isPlay: false,
      duration: 12, // 播放时长
      audio_ani: audio_ani,
      audio_stop: audio_stop
    };
  },

  created() {
    this.initPlay();
  },

  methods: {
    initPlay() {
      this.audio = new Audio();
      this.audio.src = this.audioSrc;
      this.audio.addEventListener("canplaythrough", () => {
        this.duration = parseInt(this.audio.duration);
      });

      this.audio.addEventListener("playing", () => {
        this.isPlay = true;
      });
      this.audio.addEventListener("pause", () => {
        this.isPlay = false;
      });

      // 进度事件监听
      this.audio.addEventListener("timeupdate", () => {
        this.duration = parseInt(this.audio.currentTime);
      });
      window.audioBarList = window.audioBarList || [];
      window.audioBarList.push(this.audio); // 所有实例加入全局变量
    },
    play() {
      if (this.audio.paused) {
        window.audioBarList.forEach(audio => {
          // 停止其它实力并重置时间
          audio.pause();
          audio.currentTime = 0;
        });
        this.audio.play();
      } else {
        this.audio.pause();
        this.audio.currentTime = 0;
      }
    }
  }
};
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
#audio {
  display: none;
}
.audio-bar {
  display: inline-block;
  width: 150px;
  height: 30px;
  background: #44b549;
  border-radius: 5px;
  position: relative;
  overflow: hidden;
  cursor: pointer;
  transition: all 0.4s;
  text-align: left;
  &:hover {
    background: lighten(#44b549, 10%);
  }
  .time {
    display: inline-block;
    color: #fff;
    padding-left: 10px;
    line-height: 30px;
  }
  .play-ani {
    width: 30px;
    height: 30px;
    overflow: hidden;
    float: right;
    position: relative;
    top: 3px;
    background-size: 25px 25px;
  }
}
</style>