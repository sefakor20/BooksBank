<template>
  <nav class="relative flex flex-wrap items-center justify-between py-3 mb-3">
    <div class="container mx-auto flex flex-wrap items-center justify-between">
      <h1 class="relative flex justify-between w-auto static block justify-start">
        <router-link :to="{ name: 'welcome' }">
          <img src="/assets/img/Logo-2-colour.svg" :alt="appName">
        </router-link>
      </h1>
      <button class="md:hidden rounded-full bg-white py-3 px-2 items-center text-xs uppercase font-bold leading-snug text-white text-gray-700 hover:opacity-75" @click="toggleNavbar">
        Menu
        <fa icon="bars" fixed-width />
      </button>
      <!-- desktop view -->
      <div class="hidden md:flex md:flex-grow md:items-center">
        <AuthControls class="flex flex-col md:flex-row list-none ml-auto" :authenticated="!!user" @logout="onLogout" />
      </div>
    </div>
    <!-- mobile view -->
    <div :class="{'hidden': !showMenu}" class="border-t-2 border-gray-600 w-full my-4">
      <AuthControls class="mt-2 px-2 pt-2 pb-3 space-y-1" :authenticated="!!user" @logout="onLogout" />
    </div>
  </nav>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'

import AuthControls from './Navbar/authControls.vue'

export default {
  components: {
    AuthControls
  },

  data: () => ({
    appName: window.config.appName,
    showMenu: false
  }),

  computed: mapGetters({
    user: 'auth/user'
  }),

  methods: {
    ...mapActions('auth', ['logout']),
    toggleNavbar: function () {
      this.showMenu = !this.showMenu
    },
    async onLogout () {
      // Log out the user.
      await this.logout()

      // Redirect to login.
      this.$router.push({ name: 'login' })
    }
  }
}
</script>

<style scoped>
</style>
