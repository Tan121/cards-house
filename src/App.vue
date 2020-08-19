<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <v-container>
        <div class="cards-wrap">
          <Card
                  v-for="(card, index) of cards"
                  :key="index"
                  :card="card"
                  @updateCards="updateCards"
          />
        </div>
      </v-container>
    </v-main>

    <v-footer padless>
      <v-col class="text-center">2020</v-col>
    </v-footer>
  </v-app>
</template>

<script>
import Card from './components/Card';
import axios from 'axios'

export default {
  name: 'App',
  components: {
    Card,
  },
  data () {
    return {
      cards: []
    }
  },
  methods: {
    updateCards (data) {
      this.cards[data.id - 1] = data
    },
    getCards () {
      axios({
        method: 'get',
        url: 'http://localhost:3000/cards/'
      })
        .then((response) => {
          this.cards = response.data
        })
        .catch((error) => {
          console.error(error)
        })
    }
  },
  mounted () {
    this.getCards()
  }
};
</script>

<style lang="scss">
  .cards-wrap {
    display: grid;
    gap: 30px;
    grid-template-columns: repeat(3, calc(33% - 20px));
    padding: 30px 50px;
  }

  @media (max-width: 1263px) {
    .cards-wrap {
      grid-template-columns: repeat(2, calc(50% - 15px));
    }
  }

  @media (max-width: 767px) {
    .cards-wrap {
      gap: 20px;
      grid-template-columns: 100%;
      padding: 30px 5px;
    }
  }
</style>
