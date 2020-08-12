<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <site-title :title="site.title"></site-title>
      <v-spacer/>
      <v-btn icon @click="save"><v-icon>mdi-check</v-icon></v-btn>
      <v-btn icon @click="read"><v-icon>mdi-numeric</v-icon></v-btn>
      <v-btn icon @click="readOne"><v-icon>mdi-one-up</v-icon></v-btn>
    </v-app-bar>

    <v-navigation-drawer app v-model="drawer" temporary>

    <site-menu :items="site.menu"></site-menu>
    </v-navigation-drawer>

    <v-main>
      <router-view></router-view>
    </v-main>
    <site-footer :footer="site.footer"></site-footer>
  </v-app>
</template>

<script>
import SiteTitle from '@/views/site/title'
import SiteFooter from '@/views/site/footer'
import SiteMenu from '@/views/site/menu'

export default {
  components: { SiteTitle, SiteFooter, SiteMenu },
  name: 'App',
  data: function () {
    return {
      drawer: false,
      site: {
        menu: [
          {
            title: 'home',
            active: false,
            icon: 'mdi-home',
            subItems: [
              {
                title: 'Dashboard',
                to: '/'
              }
            ],
            to: '/'
          },
          {
            title: 'about',
            active: true,
            icon: 'mdi-account',
            subItems: [
              {
                title: 'xxx'
              }
            ],
            to: '/about'
          }
        ],
        title: '타이틀입니다.',
        footer: '푸터입니다.'
      }

    }
  },
  created: function () {
    this.subscribe()
  },

  methods: {
    subscribe () {
      this.$firebase.database().ref().child('site').on('value', (sn) => {
        const v = sn.val()
        if (!v) {
          this.$firebase.database().ref().child('site').set(this.site)
          return
        }
        this.site = v
      }, (e) => {
        console.log(e.message)
      })
    },

    save: function () {
      console.log('save 000')
      this.$firebase.database().ref().child('abcd').set({
        title: 'abcd', text: 'ttttt'
      })
    },
    read: function () {
      this.$firebase.database().ref().child('abcd').on('value', (sn) => {
        console.log(sn)
        console.log(sn.val())
      })
    },
    readOne: async function () {
      const sn = await this.$firebase.database().ref().child('abcd').once('value')
      console.log(sn.val())
    }
  }
}
</script>
