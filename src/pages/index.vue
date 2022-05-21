<template>
  <section class="section">
    <div class="container" v-if="loggedInUser.msg.user.edis">
      <h1 class="title">My power consumption</h1>
      <hr>
      <h2 class="subtitle">List of supplies</h2>

      <div class="columns is-desktop is-multiline">
        <div class="column is-one-third" v-for="supply in supplies">
          <Supply :supply="supply"/>
        </div>
      </div>
      <hr>
      <span class="button is-small is-light " @click="getSupplies"
            v-bind:class="{ 'is-loading': loading }">
        <span><FontAwesomeIcon :icon="['fas', 'arrows-rotate']"/> Reload</span>
      </span>

    </div>
  </section>
</template>


<script>
import {mapGetters} from 'vuex'
import ExampleChart from "~/components/Chart/ExampleChart";
import WeeklyChartByPeriod from "~/components/Chart/WeeklyChartByPeriod";

export default {
  data: () => ({
    supplies: [],
    loading: false,
  }),
  middleware: ["auth"],
  computed: {
    ...mapGetters(['loggedInUser'])
  },
  methods: {
    loadSupplies: function () {
      this.$axios.$get('/supply/all/')
        .then(response => {
          this.supplies = response.msg
        });
    },
    getSupplies: function() {
      this.loading = true
      this.$axios.$get('/edis/supplies/')
        .then(response => {
          this.loading = false
          this.status = 1
          this.loadSupplies()
        }).catch(e => {
        this.loading = false
        this.status = -1
      })
    }
  },
  mounted: function () {
    this.loadSupplies()
  },
}
</script>
<style scoped>
.bar-chart {
  width: 80%;
  height: 80%;
  margin: auto;
  margin-top: 30px;
}
</style>
