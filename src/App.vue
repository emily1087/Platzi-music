<template lang='pug'>
  #app
    pm-header
    pm-loader(v-show="isLoading")
    section.section(v-show="!isLoading")
      nav.nav.has-shadow
        .container
          input.input.is-large(type="tex" placeholder="Buscar canciones", v-model="searchQuery")
          a.button.is-info.is-large(@click="search") Buscar
          a.button.is-danger.is-large &times;
      .container
        p
          small {{searchMessage}}
      .container.resutls
        .columns.is-multiline
          .column.is-one-quarter(v-for="track in tracks")
            pm-track(:track="track")
    pm-footer
</template>

<script>
import trackService from '@/services/track.js'
import PmFooter from '@/components/layout/footer.vue'
import PmHeader from '@/components/layout/header.vue'

import PmTrack from '@/components/Track.vue'
import PmLoader from '@/components/search/loader.vue'
export default {
  name: 'app',
  components: { PmFooter, PmHeader, PmTrack, PmLoader },
  data () {
    return {
      searchQuery: '',
      tracks: [],
      isLoading: false
    }
  },
  computed: {
    searchMessage () {
      return `Encontrados: ${this.tracks.length}`
    }
  },
  methods: {
    search () {
      if (!this.searchQuery) { return }
      this.isLoading = true
      trackService.search(this.searchQuery)
        .then(res => {
          this.tracks = res.tracks.items
          this.isLoading = false
        })
    }
  }
}
</script>

<style lang="stylus">
@import './stylus/style.styl'
  .resutls
    margin-top 30px
</style>
