<template>
  <div class="imusic-tape-wrapper">
    <div class="imusic-tape">
      <div class="imusic-tape-back" v-if="paused === true">
        <div class="imusic-tape-wheel imusic-tape-wheel-left"><div></div></div>
        <div class="imusic-tape-wheel imusic-tape-wheel-right"><div></div></div>
      </div>
      <div class="imusic-tape-back" v-else>
        <div class="imusic-tape-wheel imusic-tape-wheel-left clockwise"><div></div></div>
        <div class="imusic-tape-wheel imusic-tape-wheel-right anticlockwise"><div></div></div>
      </div>
      <div class="imusic-tape-front imusic-tape-side-a">
        <span>A</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'tape',
  props: ['paused']
}
</script>

<style scoped>
  .imusic-tape-wrapper {
    -webkit-perspective: 800px;
    -moz-perspective: 800px;
    -o-perspective: 800px;
    -ms-perspective: 800px;
    perspective: 800px;
  }
  .imusic-tape {
    width: 586px;
    height: 379px;
    margin: 30px auto 0;
    position: relative;
    -webkit-transition: all .4s ease-in-out;
    -moz-transition: all .4s ease-in-out;
    -o-transition: all .4s ease-in-out;
    -ms-transition: all .4s ease-in-out;
    transition: all .4s ease-in-out;
  }
  .imusic-tape-back {
    width: 100%;
    height: 100%;
    position: relative;
    background: transparent url(../assets/images/cs_back.png) no-repeat center center;
  }
  .imusic-tape-wheel {
    width: 125px;
    height: 125px;
    position: absolute;
    top: 110px;
    background: transparent;
    border-radius: 50%;
  }
  .imusic-tape-wheel-left {
    left: 109px;
    box-shadow: 0 0 0 70px #000;
  }
  .imusic-tape-wheel-right {
    right: 113px;
  }
  .imusic-tape-wheel div {
    width: 100%;
    height: 100%;
    background: transparent url(../assets/images/cs_wheel.png) no-repeat center center;
  }
  .imusic-tape-front {
    width: 100%;
    height: 100%;
    position: absolute;
    background: transparent url(../assets/images/cs_front.png) no-repeat center center;
    top: 0px;
    left: 0px;
  }

  .imusic-tape-front span {
    color: rgba(0, 0, 0, 0.6);
    position: absolute;
    top: 83px;
    left: 67px;
    font-family: Arial;
    font-weight: bold;
    font-size: 20px;
  }

  .clockwise {
    animation:clockwise 3s linear infinite;
  }

  .anticlockwise {
    animation:anticlockwise 3s linear infinite;
  }

  @keyframes clockwise {
    0%  {
      transform:rotate(0deg);
    }
    100% {
      transform:rotate(360deg);
    }
  }

  @keyframes anticlockwise {
    0%  {
      transform:rotate(0deg);
    }
    100% {
      transform:rotate(-360deg);
    }
  }
</style>
