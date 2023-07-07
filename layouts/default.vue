<template>
  <v-app dark >
    <v-app-bar
      fixed
      app
      color="black"
    >
      <v-toolbar-title style="margin-left:20px">{{ title }}</v-toolbar-title>
      
      <v-list color="black" style="display: flex; flex-direction: row;">
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title >{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <v-spacer></v-spacer>

      <a v-if="username">Welcome, {{ username }}</a>
      <a v-else></a>

      <v-list color="black" style="display: flex;  margin-left:40px; flex-direction: row;">
        <v-list-item
          v-for="(item, i) in items2"
          :key="i"
          link
          @click="username ? dialog = true : login()"
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>

            <v-list-item-title v-if="username">
           <v-list-item-title type="submit" @click="logout()"> Logout </v-list-item-title>
            </v-list-item-title>

            <v-list-item-title v-else>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

    </v-app-bar>
    <v-main class="custom">
        <Nuxt></Nuxt>
    </v-main>
    <v-footer
      :absolute="!fixed"
      app
    >
    <div style="margin-left:1750px">
        <v-icon href="https://www.facebook.com">mdi-facebook</v-icon>
        <v-icon style="margin-left:10px" href="https://www.twitter.com">mdi-twitter</v-icon>
        <v-icon style="margin-left:10px" href="https://www.youtube.com">mdi-youtube</v-icon>
      </div>
      <!-- <span>&copy; Version: {{version }}</span> -->
    </v-footer>
  </v-app>
</template>

<script lang="ts">
//import ver from '../package.json'
import Vue from 'vue'

export default Vue.extend({
  name: 'DefaultLayout',
  data () {
    return {
      username: "" as string | null,
      dialog: false,
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-home',
          title: 'Welcome',
          to: '/',
        },
        {
          icon: 'mdi-gamepad-variant',
          title: 'Game',
          to: '/game',
        },
        {
          icon: 'mdi-newspaper',
          title: 'News',
          to: '/news',
        },
        {
          icon: 'mdi-plus-circle-outline',
          title: 'Add Game',
          to: '/add',
        },
        /* {
          icon: 'mdi-newspaper',
          title: 'News2',
          to: '/news2-1',
        } */
      ],
      items2: [
        {
          icon: 'mdi-account-multiple',
          title: 'Login',
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Game Store'
    }
  },
  async created() {
    this.username = localStorage.getItem("username")
  },  
  methods:{
    logout(){
      const Swal = require('sweetalert2')
      Swal.fire({
      title: 'Do you want to Logout',
      icon: 'warning',
      showDenyButton: true,
      confirmButtonText: 'Yes',
      denyButtonText: `Cancel`,
      }).then((result: { isConfirmed: any;}) => {
       if (result.isConfirmed) {
        Swal.fire('Logged out!', '', 'success')
        localStorage.setItem("username","");
        this.username = "";
       } 
      })
    },
    login(){
      this.$router.push({name:"login"});
    }
  }
})
</script>

<style scoped>
.custom {
  /* background: linear-gradient(to bottom right, #080B5D, #641513); */
  background: rgb(24, 23, 23);
  
}

</style>