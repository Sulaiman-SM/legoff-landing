<template>
    <div v-if="getTransitionPhase < 4" class="loading">
      <svg class="loading-container"viewBox="25 25 50 50" >
        <circle id="loading-circle" cx="50" cy="50" r="20" fill="none" stroke="#F8E71C" stroke-width="3" />
      </svg>
      <div class="loading-background"></div>
    </div>
</template>

<script>
import { mapGetters } from 'vuex'

import anime from 'animejs'

export default {
  methods: {
    startLoadingCircle () {
      const timeline = anime.timeline()

      timeline
        .add({
          targets: this.$el.querySelector('#loading-circle'),
          easing: [0.7, 0, 0.3, 1],
          strokeDashoffset: [120, 0],
          duration: 800
        })
        .add({
          targets: this.$el.querySelector('#loading-circle'),
          easing: [0.7, 0, 0.15, 1],
          scale: 0,
          strokeWidth: 50,
          duration: 300
        })
    }
  },
  computed: {
    ...mapGetters([
      'windowWidth',
      'getTransitionPhase'
    ])
  },
  mounted () {
    if (this.getTransitionPhase === 0) {
      this.startLoadingCircle()
    }
  }
}
</script>



<style lang="scss" scoped>

.loading {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
  pointer-events: none;
}

.loading-container {
  width: 40px;
  height: 40px;
  transform: rotate(-90deg);
  position: relative;
}

.loading-background {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: -1;
}


#loading-circle {
  // animation: dash 1s cubic-bezier(0.7,-0, 0.15, 1) 1 forwards,
  //            out .3s cubic-bezier(0.7,-0, 0.15, 1) .8s 1 forwards;
  stroke-dashoffset: 125;
  stroke-dasharray: 125;
  transform-origin: center;
  display: inline-block;
  transform-box: fill-box!important;
  transform-origin: 50% 50%;
  // -moz-transform-origin: 100% 100%!important; // firefox somehow wants the origin to be like this ¯\_(ツ)_/¯
}

@keyframes jumpOut {
  0% {
    transform: scale(1);
  }
  99% {
    transform: scale(0);
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

@keyframes dash {
  0% {
    stroke-dasharray: 1, 200;
    stroke-dashoffset: 0;
    stroke: white;
  }
  90%, 100% {
    stroke-dasharray: 128, 200;
    stroke-dashoffset: 0;
    stroke: #F8E71C;
  }
}
@keyframes out {
  0% {
    // stroke-dasharray: 128, 128;
    // stroke-dashoffset: 256;
    transform: scale(1);
    stroke-width: 3;
  }
  100% {
    // stroke-dasharray: 0, 600;
    // stroke-dashoffset: 0;
    // stroke: white;
    transform: scale(0);
    stroke-width: 40;
  }
}
</style>
