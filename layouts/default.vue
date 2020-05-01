<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      disable-resize-watcher
      app
    >
      <v-text-field
        append-icon="mdi-mic"
        flat
        hide-details
        label="Search"
        prepend-inner-icon="mdi-search"
        solo-inverted
      ></v-text-field>

      <v-list
        v-for="(item, i) in navBar"
        :key="i"
      >
        <v-list-item
          :to="item.to"
          link
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      app
      color="indigo"
      dark
    >
      <v-toolbar-title>Application</v-toolbar-title>

      <v-toolbar-items>
        <v-btn
          :to="item.to"
          text
          v-for="(item, i) in navBar"
          :key="i"
          class="hidden-sm-and-down ml-4"
        >
          {{ item.title }}
        </v-btn>
      </v-toolbar-items>

      <v-spacer></v-spacer>

      <v-text-field
        append-icon="mdi-mic"
        class="hidden-sm-and-down"
        flat
        hide-details
        label="Search"
        prepend-inner-icon="mdi-search"
        solo-inverted
      ></v-text-field>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" class="hidden-md-and-up"/>
    </v-app-bar>

    <v-content>
      <v-container
        fluid
      >
        <nuxt />
      </v-container>
    </v-content>
    <v-footer
      color="indigo"
      app
    >
      <span class="white--text">&copy; 2019</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  props: {
    source: String
  },
  data: () => ({
    drawer: false,
    navBar: [
      { icon: 'mdi-home', title: 'Home', to: '/' },
      { icon: 'mdi-home', title: 'Article', to: '/articles' }
    ]
  })
}
</script>
