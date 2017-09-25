<template>
  <section class="cards-wrapper">
    <h1 class="title">Which Kind Of Salad Are You?</h1>
    <div v-if="cards" class="cards">
      <card v-for="card in cards" :card="card" :key="card.id" :style="cardStyle" />
    </div>
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
          image: 'http://placekitten.com/800/800',
          back: 'This is the back of the card'
        },
        {
          id: 2,
          image: 'http://placekitten.com/1200/400',
          back: 'This is the back of the card'
        },
        {
          id: 3,
          front: 'Cards can be pictures or text',
          back: 'and can be flipped'
        },
        {
          id: 4,
          front: 'This is the front of the card',
          back: 'This is the back of the card'
        },
        {
          id: 5,
          front: 'This is the front of the card',
          back: 'This is the back of the card'
        },
        {
          id: 6,
          front: 'This is the front of the card',
          back: 'This is the back of the card'
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

  will-change: transform;

  width: 80vw;
  max-height: 100vh;
  margin: 0 auto;
}

.title {
  color: #fff;
  font-size: 2em;
  margin: 0;
}
</style>
