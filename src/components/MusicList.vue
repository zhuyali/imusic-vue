<template>
  <div id="music-list">
    <ol>
      <li v-for="value in musicList" class="item" :data-path="value.path" @click="playSpecificMusic">{{ value.name }}</li>
    </ol>
  </div>
</template>

<script>
  export default {
    name: 'musicList',
    props: ['musicList'],
    methods: {
      changeMusic: function (index) {
        this.$emit('changeMusic', index)
      },
      playSpecificMusic: function (event) {
        for (var i in this.musicList) {
          if (this.musicList[parseInt(i)].path ===
            event.target.getAttribute('data-path')) {
            let index = parseInt(i)
            this.$emit('changeMusic', index)
            this.$emit('pressPause', false)
            break
          }
        }
        this.changeState(false, false)
      },
      pressPause: function (pressed) {
        this.$emit('pressPause', pressed)
      },
      changeState: function (paused, stopped) {
        this.$emit('changeState', paused, stopped)
      }
    }
  }
</script>

<style scoped>
  .item {
    position: relative;
    display: block;
    padding: .4em .4em .4em 2em;
    margin: .5em 0;
    background: #ddd;
    color: #444;
    text-decoration: none;
    border-radius: 1em;
    transition: all .3s ease-out;
    text-align: left;
    cursor: default;
  }

  .item:hover {
    background: #eee;
  }

  .item:hover:before {
    transform: rotate(360deg);
  }

  .item:before {
    content: counter(li);
    counter-increment: li;
    position: absolute;
    left: -1.3em;
    top: 50%;
    margin-top: -1.3em;
    background: #87ceeb;
    height: 2em;
    width: 2em;
    line-height: 2em;
    border: .3em solid #fff;
    text-align: center;
    font-weight: bold;
    border-radius: 2em;
    transition: all .3s ease-out;
  }

  #music-list {
    overflow: auto;
    position: fixed;
    top: 15%;
    right: 9%;
    counter-reset: li; /* 初始化计数器 */
    list-style: none; /* 移除默认的计数 */
    margin-left: 65%;
    padding: 0;
    height: 456px;
    text-shadow: 0 1px 0 rgba(255,255,255,.5);
  }
</style>

