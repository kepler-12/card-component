<template>
  <div class="card" @click="flip">
    <!-- Card Front -->
    <div class="front" tabindex="-1">
      <img class="card-image" v-if="card.image" :src="card.image">
      <div v-if="card.front" class="card-content">
        <h1>{{ card.front }}</h1>
      </div>
    </div>
    <!-- End Card Back -->

    <!-- Card Back -->
    <div class="back" tabindex="-1">
      <div v-if="card.back" class="card-content">
        <h1>{{ card.back }}</h1>
      </div>
    </div>
    <!-- End Card Back -->
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
      scale: 1.3, // scale during the flip
      axis: 'Y', // axis the card "flips" over
      flipped: false,
      locked: false,
      timing: {
        duration: 900,
        iterations: 1,
        easing: 'ease-in-out',
        fill: 'forwards'
      }
    }
  },
  mounted() {
    tilt.init(this.$el, {
      reverse: true, // reverse the tilt direction
      max: 25, // max tilt rotation (degrees)
      scale: 1.2, // scale on hover
    });

    // assign the parts of the card as data
    this.front = this.$el.querySelector('.front')
    this.back = this.$el.querySelector('.back')

    // flip the card right away
    setTimeout(this.flip, 500)
  },
  methods: {
    flip() {
      if (this.locked) return;
      console.log('flip!');
      this.locked = true;

      // the transforms for the side to be hidden
      const sideOne = [
        { transform: `translateZ(-200px) rotate${this.axis}(0deg) scale(${this.scale})` },
        { transform: `translateZ(-100px) rotate${this.axis}(0deg) scale(${this.scale})`, offset: 0.15 },
        { transform: `translateZ(-100px) rotate${this.axis}(180deg) scale(${this.scale})`, offset: 0.65 },
        { transform: `translateZ(-200px) rotate${this.axis}(180deg) scale(${this.scale})` }
      ];

      // the transforms for the side to be revealed
      const sideTwo = [
        { transform: `translateZ(-200px) rotate${this.axis}(180deg) scale(${this.scale})` },
        { transform: `translateZ(-100px) rotate${this.axis}(180deg) scale(${this.scale})`, offset: 0.15 },
        { transform: `translateZ(-100px) rotate${this.axis}(360deg) scale(${this.scale})`, offset: 0.65 },
        { transform: `translateZ(-200px) rotate${this.axis}(360deg) scale(${this.scale})` }
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

      // switch to the front
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
