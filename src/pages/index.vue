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
      <hr>
      <div class="bar-chart">
        <client-only>
          <ExampleChart />
          <WeeklyChartByPeriod />
        </client-only>

      </div>
    </div>
  </section>
</template>


<script>
import {mapGetters} from 'vuex'
import ExampleChart from "~/components/Chart/ExampleChart";
import WeeklyChartByPeriod from "~/components/Chart/WeeklyChartByPeriod";

export default {
  components: {WeeklyChartByPeriod, ExampleChart},
  middleware: ["auth"],
  computed: {
    ...mapGetters(['loggedInUser'])
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
