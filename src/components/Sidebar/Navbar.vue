<template>
  <nav class="navbar is-primary has-text-weight-bold">
    <div class="container">
      <div class="navbar-brand">
        <nuxt-link class="navbar-item" to="/">
          <img src="~/static/logo.png" />

        </nuxt-link>
        <button class="button navbar-burger">
          <span></span>
          <span></span>
          <span></span>
        </button>
      </div>
      <div class="navbar-menu">
        <div class="navbar-end">
          <template v-if="isAuthenticated">
            <nuxt-link class="navbar-item" to="/profile"  v-if="isAuthenticated"> {{ loggedInUser.msg.user.name }}</nuxt-link>
            <a class="navbar-item" @click="logout">Logout</a>
          </template>
          <template v-else>
            <nuxt-link class="navbar-item" to="/register">Register</nuxt-link>
            <nuxt-link class="navbar-item" to="/login">Log In</nuxt-link>
          </template>
        </div>
      </div>
    </div>
  </nav>
</template>

<script>
import {mapGetters} from 'vuex'

export default {
  computed: {
    ...mapGetters(['isAuthenticated', 'loggedInUser'])
  },
  methods: {
    async logout() {
      await this.$auth.logout();
    },
  },
}
</script>
