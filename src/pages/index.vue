<template>
  <section class="section">
    <div class="container">
      <h1 class="title">Nuxt Auth</h1>
      <hr>
      <h2 class="subtitle">My supplies</h2>
      <div class="columns is-desktop is-multiline">
        <div class="column is-one-third" v-for="item in 7">
          <Supply/>
        </div>
      </div>
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
