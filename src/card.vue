<template>
  <div class="card" @click="flip">
    <!-- Card Front -->
    <div class="front" tabindex="-1">
      <img class="card-image" v-if="card.image" :src="card.image">
      <div v-if="card.front" class="card-content">
        <h1>{{ card.front }}</h1>
      </div>
    </div>

    <!-- Card Back -->
    <div class="back" tabindex="-1">
      <div v-if="card.back" class="card-content">
        <h1>{{ card.back }}</h1>
      </div>
    </div>
  </div>
</template>

<script>
import tilt from 'vanilla-tilt'
import card from './Card.vue'

export default {
  props: ['card'],
  name: 'card',
  data() {
    return {
      front: '',
      back: '',
      axis: 'Y',
      flipped: false,
      locked: false,
      timing: {
        duration: 800,
        iterations: 1,
        easing: 'ease-in-out',
        fill: 'forwards'
      }
    }
  },
  mounted() {
    tilt.init(this.$el, {
      reverse: true,  // reverse the tilt direction
      max: 35,     // max tilt rotation (degrees)
      perspective: 1000,   // Transform perspective, the lower the more extreme the tilt gets.
      scale: 1.2,      // 2 = 200%, 1.5 = 150%, etc..
      speed: 300,    // Speed of the enter/exit transition
      easing: "cubic-bezier(.03,.98,.52,.99)",    // Easing on enter/exit.
      transition: true,   // Set a transition on enter/exit.
      reset: true,    // If the tilt effect has to be reset on exit.
    });
    this.front = this.$el.querySelector('.front')
    this.back = this.$el.querySelector('.back')
  },
  methods: {
    flip() {
      if (this.locked) return;
      console.log('flip!');
      this.locked = true;

      const sideOne = [
        { transform: `translateZ(-200px) rotate${this.axis}(0deg) scale(1.3)` },
        { transform: `translateZ(-100px) rotate${this.axis}(0deg) scale(1.3)`, offset: 0.15 },
        { transform: `translateZ(-100px) rotate${this.axis}(180deg) scale(1.3)`, offset: 0.65 },
        { transform: `translateZ(-200px) rotate${this.axis}(180deg) scale(1.3)` }
      ];

      const sideTwo = [
        { transform: `translateZ(-200px) rotate${this.axis}(180deg) scale(1.3)` },
        { transform: `translateZ(-100px) rotate${this.axis}(180deg) scale(1.3)`, offset: 0.15 },
        { transform: `translateZ(-100px) rotate${this.axis}(360deg) scale(1.3)`, offset: 0.65 },
        { transform: `translateZ(-200px) rotate${this.axis}(360deg) scale(1.3)` }
      ];

      // Switch to the back
      if (!this.flipped) {
        console.log('switching to the back');
        this.front.animate(sideOne, this.timing);
        this.back.animate(sideTwo, this.timing).onfinish = () => {
          this.front.inert = true;
          this.back.inert = false;
          this.flipped = !this.flipped;
          this.locked = false;
          this.back.focus();
        }
      }

      if (this.flipped) {
        console.log('switching to the front!');
        this.front.animate(sideTwo, this.timing);
        this.back.animate(sideOne, this.timing).onfinish = () => {
          this.front.inert = false;
          this.back.inert = true;
          this.flipped = !this.flipped;
          this.locked = false;
          this.front.focus();
        }
      }
    }
  }
}
</script>

<style lang="scss">
.card {
  margin: 1em;
  position: relative;

  perspective: 500px;
  will-change: transform;
  cursor: pointer;
}

.card-content {
  display: block;
  z-index: 200;
}

.front,
.back {
  position: absolute;
  width: 100%;
  height: 100%;

  display: flex;
  align-items: center;
  justify-content: center;

  backface-visibility: hidden;
  overflow: hidden;
  user-select: none;

  padding: 1em;
  border-radius: 3px;
  background-color: #fff;
}

.back {
  transform: rotateY(180deg);

  background-color: #880E4F;
  color: #fff;
}

.front:focus,
.back:focus {
  outline: none;
}

.card-image {
  transform: translate3d(-50%, -50%, 0);

  position: absolute;
  top: 50%;
  left: 50%;

  height: 100%;
  width: auto;
}
</style>
