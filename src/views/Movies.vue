<template>
  <v-container>
    <v-row fill-height>
      <v-col cols="12">
        <v-row v-show="isLoading" class="fill-height ma-0" align="center" justify="center">
            <div class="text-center">
              <v-overlay>
                <v-progress-circular
                  indeterminate
                  size="80"
                  color="orange"
                ></v-progress-circular>
              </v-overlay>
            </div>
          </v-row>
         <v-row v-show="!isLoading" justify="space-around" fill-height>
            <Poster
                v-for="item of items()"
                :key="item.id"
                :src="`http://aox.hopto.org:8000/image/w342`+item.poster_path"
                :name="item.title"
                :date="item.release_date"
                aspect="2/3"
                width="132"
                :to="`/movie/`+item.id"
            /> 
          </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import Poster from '@/components/Poster.vue'

export default {
  name: 'Movies',
  data: () => ({
    isLoading: false,
    movies: [],
    filter: null
  }),
  components: {
    Poster
  }, 
  methods: {
    async load(func, params = {}){
      this.isLoading = true
      await func(params)
      this.isLoading = false
    },
    async getMovies(){
      const { data } = await this.$http.get('/movies')
      function compare(a,b){
        const dateA = new Date(a.release_date)
        const dateB = new Date(b.release_date)
        return dateB - dateA
      }
      this.movies = data.sort(compare)
    },
    items(){
      return this.filter ? this.movies.filter(m => m.title.toLowerCase().includes(this.filter)) : this.movies
    }
  },
  async mounted(){
    await this.load(this.getMovies)
    this.$root.$on('search-movies', (e) => {
          this.filter = e ? this.filter = e.toLowerCase() : null
          console.log({e, name:'movies'})
    })
  },
  beforeDestroy() {
    this.$root.$off('search-movies')
},
}
</script>
