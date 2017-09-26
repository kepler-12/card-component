<template>
  <section class="cards-wrapper">
    <h1 class="title">{{ title }}</h1>

    <transition-group name="shuffle-cards" tag="div" v-if="cards" class="cards">
      <card v-for="card in cards" :card="card" :key="card.id" :style="cardStyle" />
    </transition-group>

  </section>
</template>

<script>
import { shuffle } from 'lodash'
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
        width: 'calc(80vw / 4)',
        height: 'calc(80vw / 4)'
      }
    }
  },
  mounted() {
    [1100, 1150, 1175, 1200, 1300, 1500, 2000, 2500, 3500, 4500].forEach(e => setTimeout(this.shuffle, e))
  },
  data() {
    return {
      title: 'Which Kind Of Salad Are You?',
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
  },
  methods: {
    shuffle() {
      this.cards = shuffle(this.cards)
    }
  }
}
</script>

<style lang="scss">
.shuffle-cards-move {
  transition: transform .75s;
}

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
