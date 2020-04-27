<template>
  <v-app dark>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-btn
        icon
        nuxt
        to="/"
      >
        <v-icon>mdi-home</v-icon>
      </v-btn>
      <v-toolbar-title
        nuxt
        to="/"
        v-text="title"
      />
      <v-spacer />

      <v-btn
        color="primary"
        nuxt
        to="post"
      >
        My Post
      </v-btn>
      <v-menu
        offset-y
      >
        <template v-slot:activator="{ on }">
          <v-btn icon v-on="on">
            <v-icon>mdi-dots-vertical</v-icon>
          </v-btn>
        </template>

        <v-list>
          <nuxt-link
            to="/about"
            class="aboutlink"
          >
            <v-list-item>
              <v-list-item-title>About</v-list-item-title>
            </v-list-item>
          </nuxt-link>
        </v-list>
      </v-menu>
    </v-app-bar>
    <v-content>
      <v-container>
        <nuxt />
      </v-container>
    </v-content>
    <v-footer
      :fixed="fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }} by Johnny Zheng</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: 'inspire/inspire'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Wingkong\'s Home'
    }
  }
}
</script>
<style>
  * {
    box-sizing: border-box;
  }

  .aboutlink {
    text-decoration: none;
  }
</style>
