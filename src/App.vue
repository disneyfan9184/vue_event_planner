<template>
  <v-app>
    <nav>
      <v-app-bar app dark>
        <v-app-bar-nav-icon @click="sideNav = !sideNav" class="hidden-sm-and-up nav-item"></v-app-bar-nav-icon>
        <v-toolbar-title>
          <router-link to="/" class="link">
            <h1 class="title grey--text nav-item">DevEvents</h1>
          </router-link>
        </v-toolbar-title>
        <v-spacer></v-spacer>
        <v-toolbar-items class="hidden-xs-only">
          <v-btn
            v-for="item in menuItems"
            :key="item.title"
            :to="item.link"
            text
            class="btn-nav grey--text"
          >
            <v-icon left>{{item.icon}}</v-icon>
            {{item.title}}
          </v-btn>
          <v-btn text class="btn-nav grey--text" v-if="userIsAuthenticated" @click="onLogout">
            <v-icon left>mdi-application-export</v-icon>Logout
          </v-btn>
        </v-toolbar-items>
      </v-app-bar>

      <!-- Navigation Drawer -->
      <v-navigation-drawer
        v-model="sideNav"
        app
        class="drawer-color nav-item"
        disable-resize-watcher
      >
        <v-list>
          <v-list-item v-for="item in menuItems" :key="item.title" router :to="item.link">
            <v-list-item-icon>
              <v-icon class="white--text">{{item.icon}}</v-icon>
            </v-list-item-icon>
            <v-list-item-title class="white--text">{{item.title}}</v-list-item-title>
          </v-list-item>
          <v-list-item v-if="userIsAuthenticated" @click="onLogout">
            <v-list-item-icon>
              <v-icon class="white--text">mdi-application-export</v-icon>
            </v-list-item-icon>
            <v-list-item-title class="white--text">Logout</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-navigation-drawer>
    </nav>

    <!-- Main section -->
    <v-content>
      <router-view></router-view>
    </v-content>
  </v-app>
</template>

<script>
// import HelloWorld from "./components/HelloWorld";

export default {
  name: 'App',

  components: {},

  data: () => ({
    sideNav: false
  }),
  computed: {
    menuItems() {
      let menuItems = [
        { icon: 'mdi-face', title: 'Sign up', link: '/signup' },
        { icon: 'mdi-lock-open', title: 'Sign in', link: '/signin' }
      ];
      if (this.userIsAuthenticated) {
        menuItems = [
          {
            icon: 'mdi-account-supervisor',
            title: 'View Events',
            link: '/events'
          },
          { icon: 'mdi-calendar', title: 'Organize Event', link: '/event/new' },
          { icon: 'mdi-account', title: 'Profile', link: '/profile' }
        ];
      }
      return menuItems;
    },
    userIsAuthenticated() {
      return (
        this.$store.getters.user !== null &&
        this.$store.getters.user !== undefined
      );
    }
  },
  methods: {
    onLogout() {
      this.$store.dispatch('logout');
    }
  }
};
</script>

<style scoped>
.link {
  text-decoration: none;
}
.nav-item {
  cursor: pointer;
}
.drawer-color {
  background: #333 !important;
}
.btn-nav {
  font-size: 11px !important;
}
</style>
