<template>
  <v-app class="home">
    <v-container fluid grid-list-sm>
      <v-layout align-center justify-center row fill-height>
        <div>
          <img alt="Vue logo" src="../assets/logo.png">
          <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
          <AccountPanel />
        </div>
      </v-layout>
    </v-container>
  </v-app>
  
</template>

<script>
// @ is an alias to /src
//import HelloWorld from '@/components/HelloWorld.vue'
import AccountPanel from '@/components/AccountPanel.vue'

export default {
  name: 'home',
  components: {
    //HelloWorld,
    AccountPanel
  }
}
</script>
