<template>
  <div v-if="!seen" class="notification is-warning">
    <div class="container">
      You doesn't have any EDIS information stored in Calambre. Please, go to your <router-link to="/profile" class="button is-secondary is-small">Profile</router-link> page to set your own
      credentials. This information it's stored encrypted and any attack to database can't be enough to decrypt.
      <button class="button is-small is-light" @click="close">Close</button>
    </div>

  </div>

</template>

<script>

export default {
  data: () => ({
    seen: false,
  }),
  methods: {
    close: function () {
      this.seen = true;
    },
  },
}

</script>
