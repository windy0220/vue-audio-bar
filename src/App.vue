<template>
  <div id="app">
    <vue-audio-bar audioSrc="src/assets/12146.wav"></vue-audio-bar>
  </div>
</template>

<script>
import vueAudioBar from '../src/myPlugin/vueAudioBar/vue-audio-bar'
export default {
  components:{ //注册插件
    vueAudioBar
  },
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<style lang="scss">
#app {
  color: #2c3e50;
  margin-top: 20px;
}
</style>
