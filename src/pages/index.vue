<template>
  <section class="section">
    <div class="container" v-if="loggedInUser.edis">
      <h1 class="title">My power consumption</h1>
      <hr>
      <h2 class="subtitle">List of supplies</h2>
      <div class="columns is-desktop is-multiline">
        <div class="column is-one-third" v-for="supply in supplies">
          <Supply :supply="supply"/>
        </div>
      </div>
    </div>
    <div class="container" v-else>
      <div class="columns">
        <div class="column is-half">
          <article class="message is-info">
            <div class="message-header">
              Hi!
            </div>

            <div class="message-body">
              You doesn't have any EDIS information stored in Calambre. Please, go to your Profile page to set your own
              credentials. This information it's stored encrypted and any attack to database can't be enough to decrypt.
            </div>
          </article>
        </div>
      </div>

    </div>
  </section>
</template>


<script>
import {mapGetters} from 'vuex'
import ExampleChart from "~/components/Chart/ExampleChart";
import WeeklyChartByPeriod from "~/components/Chart/WeeklyChartByPeriod";

export default {
  data: () => ({
    supplies: []
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
