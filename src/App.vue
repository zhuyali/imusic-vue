<template>
  <div id="app">
    <audio ref="audio" id="audio"><span>HTML5 audio not supported</span></audio>
    <tape :paused="paused"/>
    <panel />
    <musicList :musicList="musicList"/>
  </div>
</template>

<script>
import Vue from 'vue'
import Tape from './components/Tape'
import Panel from './components/Panel'
import MusicList from './components/musicList'

Vue.use(require('vue-resource'))

export default {
  name: 'app',
  data: function () {
    return {
      paused: true,
      stopped: false,
      musicList: [{name: 'aaa', path: 'sss'}]
    }
  },
  components: {
    Tape,
    Panel,
    MusicList
  },
  mounted: function () {
    let that = this
    Vue.http.get('/api/get_musics')
      .then(response => response.json())
      .then(result => {
        if (result.success) {
          that.musicList = result.data
          console.log(that.musicList)
        }
      })
  }
}
</script>

<style>
#audio {
  display: none;
}
</style>
