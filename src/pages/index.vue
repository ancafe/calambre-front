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
