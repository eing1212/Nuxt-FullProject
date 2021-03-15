<template>
  <v-app light>
    <v-navigation-drawer
      v-model="drawer"
      color="bulbul"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
      >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          class="bl--text"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar color="bulbul" :clipped-left="clipped" fixed app class="banner white--text">
      <v-toolbar-title v-text="title" />
      <v-btn icon @click.stop="miniVariant = !miniVariant">
        <v-icon class="gray--text"
          >mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon
        >
      </v-btn>
      <v-app-bar-nav-icon class="gray--text" @click.stop="drawer = !drawer" />
      <v-spacer />
      <v-btn class="buttonS" color="banner" @click="logout()">Log out</v-btn>
    </v-app-bar>
    <v-main class="mainS">
      <v-container>
      <nuxt />
      </v-container>
    </v-main>
    <v-footer class="foot banner--text" :absolute="!fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
import firebase from 'firebase/app'
export default {
  data() {
    return {
      login: true,
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-cart-variant',
          title: 'Shop',
          to: '/ShopEmployee',
        },
        {
          icon: 'mdi-cart-arrow-down',
          title: 'Order',
          to: '/order',
        },
         {
           icon: 'mdi-credit-card-outline',
           title: 'Payment',
           to: '/payment',
         },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'SKYCAR RENTAL',
    }
  },
  beforeCreate() {
    if (!firebase.auth().currentUser) {
      console.log('No Login')
      this.$router.replace('/')
    } else {
      console.log('Login ok')
    }
  },
  methods: {
    logout() {
      firebase
        .auth()
        .signOut()
        .then(() => {
          // Sign-out successful.
          console.log('Signout')
          this.$router.replace('/')
        })
        // eslint-disable-next-line handle-callback-err
        .catch((error) => {
          console.log('An error happened.')
          // An error happened.
        })
    },
  },
}
</script>

<style>
.theme--dark.v-application {
  background: rgb(198, 244, 250);
  background: linear-gradient(
    38deg,
    rgb(163, 226, 241) 3%,
    rgb(86, 159, 196) 63%
  );
}

.bulbul {
  background: rgb(114, 238, 255);
  background: linear-gradient(
    270deg,
    rgba(114, 238, 255, 1) 16%,
    rgba(59, 133, 226, 1) 94%
  );
}
/* .theme--light.v-sheet {
  background-color: #252525;
  border-color: #7a7676;
  color: #ffff;
} */
.blublack {
  background: rgb(0, 0, 0);
  background: linear-gradient(
    38deg,
    rgba(0, 0, 0, 1) 3%,
    rgba(25, 91, 125, 1) 63%
  );
}

</style>
