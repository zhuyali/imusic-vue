<template>
  <div>
    <audio ref="audio" id="audio"><span>HTML5 audio not supported</span></audio>
    <tape :paused="paused"/>
    <panel v-model="paused, stopped"/>
    <musicList :musicList="musicList"/>
  </div>
</template>

<script>
import Tape from './components/Tape'
import Panel from './components/Panel'
import MusicList from './components/musicList'
import 'vue-resource'

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
    that.$http.get('/api/get_musics').then(response => {
      response.json()
    }).then(result => {
      if (result.success) {
        that.musicList = result.data
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
