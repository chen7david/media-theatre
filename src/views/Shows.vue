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
                :name="item.name"
                :date="item.first_air_date"
                aspect="2/3"
                width="132"
                :to="`/show/`+item.id"
            /> 
          </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import Poster from '@/components/Poster.vue'

export default {
  name: 'Home',
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
      const { data } = await this.$http.get('/shows')
      function compare(a,b){
        const dateA = new Date(a.first_air_date)
        const dateB = new Date(b.first_air_date)
        return dateB - dateA
      }
      this.movies = data.sort(compare)
    },
    items(){
      return this.filter ? this.movies.filter(m => m.name.toLowerCase().includes(this.filter)) : this.movies
    }
  },
  async mounted(){
    await this.load(this.getMovies)
    this.$root.$on('search-shows', (e) => {
          this.filter = e ? this.filter = e.toLowerCase() : null
    })
  },
  beforeDestroy() {
    this.$root.$off('search-shows')
},
}
</script>
