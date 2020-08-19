<template>
  <v-card>
    <v-img
      :src="card.src"
      class="white--text align-end"
      gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
      height="300px"
    >
      <v-card-title v-text="card.title"></v-card-title>
    </v-img>

    <v-card-actions>
      <v-spacer></v-spacer>

      <v-btn icon @click="addLike">
        <v-icon :class="{'active': like === 'true'}">mdi-heart</v-icon>
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
  import axios from "axios";

  export default {
    name: 'Card',
    props: ['card'],
    data () {
      return {
        like: 'false'
      }
    },
    methods: {
      addLike () {
        this.like = this.like === 'true' ? 'false' : 'true'
        const searchParams = new URLSearchParams()
        searchParams.append('title', this.card.title)
        searchParams.append('src', this.card.src)
        searchParams.append('like', this.like)

        axios({
          method: 'put',
          url: `http://localhost:3000/cards/${this.card.id}`,
          data: searchParams
        })
                .then((response) => {
                  this.$emit('updateCards', response.data)
                })
                .catch((error) => {
                  console.error(error)
                })
      }
    },
    mounted() {
      this.like = this.card.like
    }
  }
</script>

<style lang="scss">
  .active {
    color: red !important;
  }
</style>
