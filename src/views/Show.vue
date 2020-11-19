<template>
    <v-container fill-height>
        <v-row>
          <v-col cols="12">
            <v-row justify="center"> 
                <v-col align="center" lg="9" sm="12" xs="12">
                  <Details
                    :width="size"
                    :name=" episode.name || item.name"
                    :date="item.first_air_date"
                    
                    textlimit="0"
                  />
                  <media-player
                      :baseURL="$config.api.file+`/shows`"
                      :videos="episode.videos"
                      :subtitles="episode.subtitles"
                      :poster="$config.api.meta+`/image/w1280`+item.backdrop_path"
                      aspect="16/9"
                      :width="size"        
                  />
                  <SeasonsTable @selected="setEpisode" align="center" :item="item" :width="size"/>
                </v-col>
              </v-row>
          </v-col>
        </v-row>
    </v-container>
</template>

<script>
import MediaPlayer from './../components/MediaPlayer';
import Details from './../components/Details';
import SeasonsTable from './../components/SeasonsTable';


export default {
  name: 'Movies',

  components: {
    'media-player' : MediaPlayer,
    Details,
    SeasonsTable
  },

  props: {
    id: null
  },

  data: () => ({
    item: {},
    windowWidth: null,
    episode: {}
  }),

  computed: {
    size(){
      return this.windowWidth * 0.8
    }
  },
  methods: {
    async getShows(){
      const  { data }  = await this.$http.get(`/shows/${this.id}`)
      const seasons = data.seasons.filter(s => s.episodes.length > 0)
      this.episode = seasons[0].episodes[0]
      data.seasons = seasons
      this.item = data
    },
 
    setScreen(){
      this.windowWidth = this.$vuetify.breakpoint.width
    },
    setEpisode(ep){
      this.episode = ep
    }
  },
  async mounted(){
    await this.getShows()
    this.setScreen()
  },
  created(){
    window.addEventListener('resize',this.setScreen)
  }
};
</script>