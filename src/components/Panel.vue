<template>
  <ul class="imusic-controls">
    <li id="play" class="imusic-control-play control" v-bind:class="{ 'imusic-control-pressed': playPressed }" @click="handleClick">
    Play<span>▶</span>
    </li>
    <li id="pause" class="imusic-control-pause control" v-bind:class="{ 'imusic-control-pressed': pausePressed }" @click="handleClick">
    Pause<span>❚❚</span>
    </li>
    <li id="pre" class="imusic-control-rewind control" v-bind:class="{ 'imusic-control-pressed': prePressed }" @click="handleClick">
    Pre<span>◁</span>
    </li>
    <li id="next" class="imusic-control-fforward control" v-bind:class="{ 'imusic-control-pressed': nextPressed }" v-on:click="handleClick">
    Next<span>▷</span>
    </li>
    <li id="stop" class="imusic-control-stop control" v-bind:class="{ 'imusic-control-pressed': stopPressed }" @click="handleClick">
    Stop<span>◼</span>
    </li>
    <audio id="soundAudio" ref="audio" src='src/assets/sounds/click.mp3'><span>HTML5 audio not supported</span></audio>
  </ul>
</template>

<script>
  export default {
    name: 'panel',
    data: function () {
      return {
        playPressed: false,
        prePressed: false,
        nextPressed: false,
        stopPressed: false
      }
    },
    props: ['paused', 'index', 'pausePressed', 'musicLength'],
    methods: {
      changeState: function (paused, stopped) {
        this.$emit('changeState', paused, stopped)
      },
      changeMusic: function (index) {
        this.$emit('changeMusic', index)
      },
      pressPause: function (pressed) {
        this.$emit('pressPause', pressed)
      },
      handleClick: function (event) {
        var audio = this.$refs.audio
        audio.play()

        let that = this
        switch (event.target.id) {
          case 'play':
            that.playPressed = true
            setTimeout(function () {
              that.playPressed = false
            }, 100)
            that.pressPause(false)
            that.changeState(false, false)
            break
          case 'pause':
            if (that.paused) {
              that.pressPause(false)
              that.changeState(false, false)
            } else {
              that.pressPause(true)
              that.changeState(true, false)
            }
            break
          case 'pre':
            if (that.index === 0) {
              that.index = that.musicLength - 1
            } else {
              that.index = that.index - 1
            }
            that.pressPause(false)
            that.prePressed = true
            setTimeout(function () {
              that.prePressed = false
            }, 100)
            that.changeState(false, false)
            that.changeMusic(that.index)
            break
          case 'next':
            if (that.index === (that.musicLength - 1)) {
              that.index = 0
            } else {
              that.index = that.index + 1
            }
            that.pressPause(false)
            that.nextPressed = true
            setTimeout(function () {
              that.nextPressed = false
            }, 100)
            that.changeState(false, false)
            that.changeMusic(that.index)
            break
          case 'stop':
            that.pressPause(false)
            that.stopPressed = true
            that.changeState(true, true)
            setTimeout(function () {
              that.stopPressed = false
            }, 100)
            break
        }
      }
    }
  }
</script>

<style scoped>
  ul.imusic-controls {
    list-style: none;
    width: 540px;
    bottom: 18px;
    margin: auto auto;
    background: -moz-linear-gradient(top, rgba(170,170,170,0.35) 0%, rgba(255,255,255,0.44) 50%, rgba(255,255,255,0.53) 100%);
    background: -webkit-gradient(linear, left top, left bottom, color-stop(0%,rgba(170,170,170,0.35)), color-stop(50%,rgba(255,255,255,0.44)), color-stop(100%,rgba(255,255,255,0.53)));
    background: -webkit-linear-gradient(top, rgba(170,170,170,0.35) 0%,rgba(255,255,255,0.44) 50%,rgba(255,255,255,0.53) 100%);
    background: -o-linear-gradient(top, rgba(170,170,170,0.35) 0%,rgba(255,255,255,0.44) 50%,rgba(255,255,255,0.53) 100%);
    background: -ms-linear-gradient(top, rgba(170,170,170,0.35) 0%,rgba(255,255,255,0.44) 50%,rgba(255,255,255,0.53) 100%);
    background: linear-gradient(to bottom, rgba(170,170,170,0.35) 0%,rgba(255,255,255,0.44) 50%,rgba(255,255,255,0.53) 100%);
    filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#59aaaaaa', endColorstr='#87ffffff',GradientType=0 );
    border: 1px solid rgba(0,0,0,0.1);
    border-bottom-color: rgba(255,255,255,0.6);
    padding: 8px;
    height: 54px;
    box-shadow:
    inset 0 1px 0px rgba(0,0,0,0.05),
    0 1px 0 rgba(255,255,255,0.8),
    0 -1px 0 rgba(255,255,255,0.4),
    inset 0 2px 19px rgba(0,0,0,0.05),
    0 2px 1px rgba(0,0,0,0.06);
    -webkit-box-sizing: content-box;
    -moz-box-sizing: content-box;
    box-sizing: content-box;
    border-radius: 12px;
  }

/* Controls list items */
  ul.imusic-controls li {
    display: block;
    float: left;
    width: 80px;
    height: 30px;
    line-height: 55px;
    text-align: left;
    padding: 10px;
    margin: 0;
    cursor: pointer;
    background: #ddd url(../assets/images/metal.jpg) no-repeat center top;
    box-shadow:
    inset 0 0 0 1px rgba(0,0,0, 0.2),
    inset 0 0 1px 2px rgba(255,255,255,0.9),
    inset 0 -6px 5px rgba(0,0,0,0.1),
    0 6px 7px rgba(0,0,0,0.3),
    0 4px 1px rgba(0,0,0,0.5);
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -khtml-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}

  #play {
    border-radius: 8px 0 0 8px;
  }

  #stop {
    border-radius: 0px 8px 8px 0px;
  }

  ul.imusic-controls li.imusic-control-play {
    width: 120px;
  }

/* Control icons */
  ul.imusic-controls li span {
    font-size: 16px;
    line-height: 30px;
    text-align: center;
    float: left;
    text-shadow:  1px 1px 1px rgba(255,255,255,0.9);
    font-style: normal;
    font-weight: normal;
    text-transform: none;
    speak: none;
    display: inline-block;
    text-decoration: inherit;
    width: 1em;
    margin-right: 0.2em;
    text-align: center;
  }

  ul.imusic-controls li:hover {
    box-shadow:
    inset 0 0 0 1px rgba(0,0,0, 0.2),
    inset 0 0 1px 2px rgba(255,255,255,0.9),
    inset 0 -10px 15px rgba(0,0,0,0.1),
    0 6px 7px rgba(0,0,0,0.3),
    0 4px 1px rgba(0,0,0,0.5);
  }

/* Pressed (active) */
  ul.imusic-controls li.imusic-control-active {
    height: 30px;
    margin-top: 2px;
    background-image: url(../assets/images/metal_dark.jpg);
    box-shadow:
    inset 0 0 0 1px rgba(0,0,0, 0.18),
    inset 0 0 1px 2px rgba(255,255,255,0.5),
    inset 0 -6px 5px rgba(0,0,0,0.1),
    0 6px 7px rgba(0,0,0,0.3),
    0 2px 1px rgba(0,0,0,0.5);
  }

/* Activated */
  ul.imusic-controls li.imusic-control-pressed,
  ul.imusic-controls li.imusic-control-active.imusic-control-pressed {
    height: 30px;
    background-image: url(../assets/images/metal_dark.jpg);
    margin-top: 4px;
    box-shadow:
    inset 0 0 0 1px rgba(0,0,0, 0.2),
    inset 0 0 5px 1px rgba(255,255,255,0.5),
    inset 0 -10px 15px rgba(0,0,0,0.2),
    0 7px 5px rgba(255,255,255,0.5);
  }

  #soundAudio {
    display: none;
  }
</style>
