<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list-item>
        <v-list-item-content>
          <v-list-item-avatar>
            <v-toolbar-title>
              <nuxt-link to="/">
                <v-img
                  class="ml-2"
                  src="https://res.cloudinary.com/sparaclet-inc/image/upload/v1687064032/sparaclet%20inc/logo_urscyx.png"
                  contain
                  width="60"
                ></v-img>
              </nuxt-link>
            </v-toolbar-title>
          </v-list-item-avatar>
        </v-list-item-content>
      </v-list-item>

      <v-list shaped class="mt-4">
        <v-list-item-group v-model="selectedItem" color="#252362">
          <v-list-item
            v-for="(item, i) in items"
            :key="i"
            class="mt-2"
            :to="item.to"
            router
            exact
          >
            <v-list-item-icon>
              <v-icon v-text="item.icon"></v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title
                v-text="item.text"
                class="titl-fnt-mb"
              ></v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-list>
      <template v-slot:append>
        <div class="pa-4">
          <v-btn text color="#828288" class="titl-fnt-mb" @click="logout">
            <v-icon left> mdi-logout </v-icon> Log out
          </v-btn>
        </div>
      </template>
    </v-navigation-drawer>
    <v-app-bar fixed app color="#FFFFFF" flat>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title
        ><span class="txtC font-weight-bold titl-fnt-mb"
          >Lovethub</span
        ></v-toolbar-title
      >

      <v-spacer></v-spacer>

      <v-badge color="red" dot class="mr-4">
        <v-icon color="#252362" small> mdi-bell </v-icon>
      </v-badge>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      clipped: false,
      drawer: true,
      fixed: true,
      selectedItem: null,
      items: [
        {
          text: 'Dashboard',
          icon: 'mdi-view-dashboard-outline',
          to: '/dashboard/',
        },
        //  { text: 'Profile', icon: 'mdi-account-group', to: '/partners/dashboard/profile'  },
        {
          text: 'Airtime',
          icon: 'mdi-phone',
          to: '/dashboard/airtime',
        },
        {
          text: 'Data',
          icon: 'mdi-cellphone',
          to: '/dashboard/data',
        },
        {
          text: 'Electricity',
          icon: 'mdi-home-lightning-bolt-outline',
          to: '/dashboard/electricity',
        },
        {
          text: 'Cable',
          icon: 'mdi-television',
          to: '/dashboard/cable',
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js',
      pagename: this.$nuxt.$route.name,
      navname: '',
    }
  },
  methods: {
    logout() {

      this.$router.push({ name: 'index' })
    },
  },
  filters: {
    formattitle(value) {
      if (value == 'partners-dashboard') {
        return 'Dashboard'
      }
      if (value == 'partners-dashboard-growth') {
        return 'Growth Plans'
      } else {
        return 'Partner Dashboard'
      }
    },
  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Urbanist:wght@500&display=swap');
.titl-fnt-mb {
  font-family: 'Urbanist', sans-serif;
  font-size: 15px;
  color: #252362;
}

#bg {
  background: #e5e5e5;
}
.txtC {
  color: #252362;
}

html {
  font-family: 'Lato', sans-serif;
}
</style>
