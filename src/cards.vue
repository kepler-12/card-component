<template>
  <section class="cards">
    <card v-for="card in cards" :card="card" :key="card.id" :style="cardStyle" />
  </section>
</template>

<script>
import tilt from 'vanilla-tilt'
import card from './Card.vue'

export default {
  name: 'cards',
  components: {
    card
  },
  computed: {
    cardStyle() {
      return {
        width: `calc(80vw / 4)`,
        height: `calc(80vw / 4)`
      }
    }
  },
  mounted() {
    tilt.init(this.$el, {
      reverse: true,
      max: 5
    });
  },
  data() {
    return {
      cards: [
        {
          id: 1,
          front: 'This is the front of the card',
          back: 'This is the back of the card'
        },
        {
          id: 2,
          front: 'This is the front of the second card',
          back: 'This is the back of the second card'
        },
        {
          id: 3,
          front: 'This is the front of the second card',
          back: 'This is the back of the second card'
        },
        {
          id: 4,
          front: 'This is the front of the second card',
          back: 'This is the back of the second card'
        },
        {
          id: 5,
          front: 'This is the front of the second card',
          back: 'This is the back of the second card'
        },
        {
          id: 6,
          front: 'This is the front of the second card',
          back: 'This is the back of the second card'
        },
      ],
    }
  }
}
</script>

<style lang="scss">
  .cards {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;

    width: 80vw;
    max-height: 100vh;
    margin: 0 auto;
  }
</style>
