<template>
    <nav>
        <v-navigation-drawer
            app
            dark
            v-model="drawer"
        >       
            <v-row justify="center" class="my-5">
                <v-avatar size="150px">
                    <img src="https://www.iconfinder.com/data/icons/business-avatar-1/512/8_avatar-512.png" alt="">
                </v-avatar>
                <v-list-item three-line class="text-center">
                    <v-list-item-content>
                        <v-list-item-title>David M.Chen</v-list-item-title>
                        <v-list-item-subtitle>Teacher</v-list-item-subtitle>
                        <v-list-item-subtitle>
                            <v-chip class="font-weight-bold" color="success">$1005</v-chip>
                        </v-list-item-subtitle>
                    </v-list-item-content>
                </v-list-item>
                <v-switch
                v-model="dark"
                color="purple"
                hide-details
                inset
            ></v-switch>
            </v-row>
            
            <v-list>
                <v-list-item
                    v-for="link of links"
                    :key="link.to"
                    router
                    :to="link.to"
                >
                    <v-list-item-action>
                        <v-icon>{{link.icon}}</v-icon>
                    </v-list-item-action>
                    <v-list-item-content>{{link.name}}</v-list-item-content>
                </v-list-item>
            </v-list>
        </v-navigation-drawer>
        <v-app-bar
            app
            dark
        >
            <v-app-bar-nav-icon @click="drawer = !drawer"/>
            <v-text-field
                v-if="isSearchable"
                class="mt-6 mx-3"
                light
                solo
                filled
                rounded
                append-inner-icon="mdi-movie"
                placeholder="Search..."
                dense
                clearable
                v-model="phrase"
            >
            </v-text-field>
            <v-spacer></v-spacer>
            <v-btn tile v-if="false" router to="/login">Login</v-btn>
            <v-btn tile router to="/register">Register</v-btn>
        </v-app-bar>
    </nav>
</template>

<script>
export default {
    name: 'Navbar',
    data: () => ({
        isLoading: false,
        drawer: false,
        dark: true,
        page: null,
        searchable: ['movies', 'shows'],
        phrase: '',
        links: [
            {
                name: 'Movies',
                icon: 'mdi-movie',
                to: '/movies'
            },
            {
                name: 'Shows',
                icon: 'mdi-monitor',
                to: '/shows'
            },
            {
                name: 'Search',
                icon: 'mdi-magnify',
                to: '/search'
            },
            {
                name: 'Documentation',
                icon: 'mdi-file',
                to: '/documentation'
            },
        ]
    }),
    computed:{
        isSearchable(){
            return this.searchable.includes(this.page)
        }
    },
    methods: {
        lookFor(){
            console.log()
        }
    },
    watch: {
      dark(dark){
        this.$vuetify.theme.dark = dark
      },
      phrase(phrase){
          this.$root.$emit(`search-${this.page}`,phrase)
      },
      $route(to){
        this.page = to.name.toLowerCase()
        this.phrase = ''
      }
    },
    created(){
        this.page = this.$route.name.toLowerCase()
    }
}
</script>