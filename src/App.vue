<template lang="pug">
#app
  img(src='./dist/logo.png')
  h1 PlatziMusic

  select(v-model="selectedCountry")
    option(v-for="country in countries" :value="country.value") {{ country.name }}

  spinner(v-show="loading")

  ul
    artist(v-for="artist in artists" :artist="artist" :key="artist.mbid")

</template>

<script>
import Spinner from './components/Spinner.vue'
import Artist from './components/Artist.vue'
import getArtists from './api'

export default {
  name: 'app',
  data () {
    return {
      loading: true,
      artists: [],
      countries: [
        {name: 'Argentina', value: 'argentina'},
        {name: 'Ecuador', value: 'ecuador'},
        {name: 'España', value: 'spain'},
      ],
      selectedCountry: 'argentina',
    }
  },
  components: {
    Artist, Spinner
  },
  methods: {
    refreshArtists(){
      const self = this
      this.loading = true
      this.artists = [];
      // then no puede ir como arrow functions porque haria referencia a window
      getArtists(this.selectedCountry)
        .then( function (artists) {
          self.artists = artists
          self.loading = false
        })

    }
  },
  mounted: function() {
    this.refreshArtists()
  },
  watch: {
  // vigila selectedCountry y cuando cambia realiza algo
    selectedCountry: function() {
      this.refreshArtists()
    }

  }
}
</script>

<style lang="stylus">
#app
    font-family 'Avenir', Helvetica, Arial, sans-serif
    -webkit-font-smoothing antialiased
    -moz-osx-font-smoothing grayscale
    text-align center
    color #2c3e50
    margin-top 60px

h1, h2
    font-weight normal

ul
    list-style-type none
    padding 0

li
    display inline-block
    margin 0 10px

a
    color #42b983
</style>
