<template>
  <nav class="navbar is-primary has-text-weight-bold">
    <div class="container">
      <div class="navbar-brand">
        <nuxt-link class="navbar-item" to="/">
          <img src="~/static/logo.png"/>

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
            <nuxt-link class="navbar-item" to="/profile" v-if="isAuthenticated">

              <span class="icon-text">
                <FontAwesomeIcon class="icon" icon="user"/>
                <span>{{ loggedInUser.msg.user.name }}</span>
              </span>

            </nuxt-link>

            <a class="navbar-item" @click="logout">
              <span class="icon-text">
                <span>Logout</span>
                <FontAwesomeIcon class="icon" icon="right-from-bracket"/>
              </span>
            </a>

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
import Fas from "~/components/Icons/Fas";

export default {
  components: {Fas},
  computed: {
    ...mapGetters(['isAuthenticated', 'loggedInUser'])
  },
  methods: {
    async logout() {
      await this.$auth.logout();
      this.$router.push('/login')
    },
  },
}
</script>
