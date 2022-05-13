<template>
  <nuxt-link :to="{ name: 'cups', params: { supply: this.$props.supply }}">
    <div class="card">
      <div class="card-content">
        <div class="media">
          <div class="media-left">
            <figure class="image is-48x48">
              <img src="~/static/electric-meter.png" alt="Electric meter">
            </figure>
          </div>


          <div class="media-content">
            <p class="title is-4"><span class="material-icons">power</span> CUPS {{ supply.cups }}</p>
            <p class="subtitle is-6">Suministro eléctrico</p>
          </div>
        </div>

        <div class="content">
          <span class="material-icons">home</span> Dirección: {{ supply.provisioning_address }}
        </div>
      </div>
    </div>
  </nuxt-link>
</template>

<script>
export default {
  name: "Supply.vue",
  props: [
    'supply'
  ]
}
</script>

<style scoped>

</style>
