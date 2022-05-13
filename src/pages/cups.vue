<template>
  <section class="section">
    <div class="container">
      <h1 class="title">My power consumption</h1>
      <hr>
      <h2 class="subtitle">Supply address: {{ supply.provisioning_address }}</h2>

      <div class="tabs is-large is-centered">
        <ul>
          <li class="is-active"><a>Daily</a></li>
          <li><a>Monthly</a></li>
          <li><a>Stationary</a></li>
          <li><a>Consumption profile</a></li>
        </ul>
      </div>


      <div class="bar-chart">
        <client-only>
          <WeeklyChartByPeriod :supply="supply"/>
          <ExampleChart/>

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
  data: () => ({
    supply: {},
  }),
  components: {WeeklyChartByPeriod, ExampleChart},
  middleware: ["auth"],
  computed: {
  },
  methods: {
  },
  mounted: function () {
    this.supply = this.$route.params.supply
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
