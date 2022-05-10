<template>
  <section class="section">
    <div class="container">
      <h2 class="title">My Profile</h2>
      <div class="content">
        <p>
          <strong>Username:</strong>
          {{ loggedInUser.msg.user.name }}
        </p>
        <p>
          <strong>Email:</strong>
          {{ loggedInUser.msg.user.email }}
        </p>
      </div>
      <button class="button is-danger is-outlined">
        <span class="icon is-small">
          <i class="fas fa-home"></i>
        </span>
        <span>Logout</span>
      </button>
    </div>
  </section>
</template>

<script>
import {mapGetters} from 'vuex'

export default {
  middleware: ["auth"],
  computed: {
    ...mapGetters(['loggedInUser'])
  }
}
</script>
