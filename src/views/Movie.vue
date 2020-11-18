<template>
    <v-container>
        <media-player
            baseURL="http://aox.hopto.org:9000/movies"
            :videos="item.videos"
            :subtitles="item.subtitles"
            :poster="`http://aox.hopto.org:8000/image/w780`+item.backdrop_path"
            aspect="16/9"
            width="500"        
        />
    </v-container>
</template>

<script>
import MediaPlayer from './../components/MediaPlayer';


export default {
  name: 'Movies',

  components: {
    'media-player' : MediaPlayer
  },

  props: {
    id: null
  },

  data: () => ({
    item: {},
  }),

  computed: {

  },
  methods: {
    async getMovie(){
      const  { data }  = await this.$http.get(`/movies/${this.id}`)
      this.item = data
    }
  },

  async mounted(){
     await this.getMovie()
  }
};
</script>