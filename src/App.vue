<template>
  <div id="app">
    <audio ref="audio" id="audio"><span>HTML5 audio not supported</span></audio>
    <tape :paused="paused" :stopped="stopped"/>
    <panel @changeState="onStateChanged" :paused="paused" :musicLength="musicLength" :index="index" @changeMusic="onMusicChanged" @pressPause="onPausePressed" :pausePressed="pausePressed" />
    <musicList @changeState="onStateChanged" :musicList="musicList" @changeMusic="onMusicChanged" @pressPause="onPausePressed" />
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
      index: 0,
      musicLength: 0,
      paused: false,
      stopped: true,
      pausePressed: false,
      musicList: []
    }
  },
  components: {
    Tape,
    Panel,
    MusicList
  },
  methods: {
    onStateChanged: function (paused, stopped) {
      var audio = this.$refs.audio
      if (!paused && !this.stopped && !stopped) {
        this.paused = false
        this.stopped = false
        audio.play()
      } else if (paused && !stopped) {
        this.paused = true
        this.stopped = false
        audio.pause()
      } else if (!this.stopped && stopped) {
        this.paused = false
        this.stopped = true
        audio.pause()
      } else if (!paused && this.stopped && !stopped) {
        this.paused = false
        this.stopped = false
        audio.load()
        audio.play()
      }
    },
    onMusicChanged: function (index) {
      var audio = this.$refs.audio
      this.index = index
      audio.setAttribute('src', this.musicList[index].path)
      document.title = this.musicList[index].name + ' - imusic'
      audio.play()
    },
    onPausePressed: function (pressed) {
      this.pausePressed = pressed
    },
    getQueryString: function (name) {
      var reg = new RegExp('(^|&)' + name + '=([^&]*)(&|$)', 'i')
      var r = window.location.search.substr(1).match(reg)
      if (r) {
        return unescape(r[2])
      }
      return null
    }
  },
  mounted: function () {
    var audio = this.$refs.audio
    let that = this
    if (document.domain === 'localhost' || document.domain === '127.0.0.1') {
      Vue.http.get('/api/get_musics')
      .then(response => response.json())
      .then(result => {
        if (result.success) {
          that.musicList = result.data
          that.musicLength = result.data.length
        }
      })
    } else {
      var url = that.getQueryString('url')
      var defaultUrl = 'http://mr1.doubanio.com/678811c82aa655a13a64dcc4d1bea8ac/0/fm/song/p2638812_128k.mp3'
      var musicListTemp = []
      musicListTemp.push({
        name: url ? url.substr(url.lastIndexOf('/') + 1) : '美好事物-房东的猫.mp3',
        path: url || defaultUrl
      })
      this.musicList = musicListTemp
      this.musicLength = 1
    }
    audio.setAttribute('src', that.musicList[0].path)
    document.title = that.musicList[0].name + ' - imusic'
    audio.addEventListener('ended', function () {
      that.index = that.index === that.musicLength - 1 ? 0 : that.index + 1
      audio.setAttribute('src', that.musicList[that.index].path)
      document.title = that.musicList[that.index].name + ' - imusic'
      audio.play()
    })
  }
}
</script>

<style>
#audio {
  display: none;
}
</style>
