<template lang="pug">
  #app
    img(src='https://smarquez-dev.github.io/platzimusic/dist/logo.png')
    h1 PlatziMusic
    h3 Selecciona el país para la lista de Artistas
    select(v-model="selectedCountry")
      option(v-for="country in countries" v-bind:value="country.value") {{ country.name }}
    h2.h2-list Lista de Artistas más escuchados en España
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
</template>

<script>
import Artist from './components/Artist.vue'
import Spinner from './components/Spinner.vue'
import getArtists from './api/index.js'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries:[
        { name: 'España', value: 'spain' },
        { name: 'Colombia', value: 'colombia' },
        { name: 'Argentina', value: 'argentina' },
        { name: 'Francia', value: 'france' },
        { name: 'Inglaterra', value: 'United Kingdom' },
        { name: 'Puerto Rico', value: 'puerto rico' },
        { name: 'Cuba', value: 'cuba' }
      ],
      selectedCountry: 'spain',
      loading: true
    }
  },
  components: {
    Artist,
    Spinner
  },
  methods: {
    refreshArtists(country) {
      const self = this
      this.loading = true
      this.artists = []
      getArtists(country)
        .then(function (artists) {
          self.loading = false
          self.artists = artists
        })
    }
  },
  mounted() {
    this.refreshArtists(this.selectedCountry);
    // const self = this
    // getArtists()
    //   .then(function (artists) {
    //     self.artists = artists
    //   })
  },
  watch: {
    selectedCountry () {
      this.refreshArtists(this.selectedCountry)
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
.h2-list
  margin 60px 0 20px 0
  color darken(#42b983, 10%)
  font-weight 500
ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>
