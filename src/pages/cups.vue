<template>
  <div>
    <section class="section">
      <div class="container">
        <div class="columns">
          <div class="column">
            <h1 class="title">My power consumption</h1>
          </div>
          <div class="column has-text-right has-text-grey-light is-size-7 ">
            <div class="has-text-weight-bold">
              Latest data received:
            </div>
            <div v-if="supply.last_data">
              {{ last_day }}
            </div>
            <div v-else>Never</div>
          </div>
        </div>

        <hr>
        <h2 class="subtitle">Supply address: {{ supply.provisioning_address }}</h2>

        <div class="tabs is-centered" v-if="!empty">
          <ul>
            <li @click="setTab('d')" :class="active_tab === 'd' ? 'is-active' : ''"><a>Daily</a></li>
            <li @click="setTab('w')" :class="active_tab === 'w' ? 'is-active' : ''"><a>Weekly</a></li>
            <li @click="setTab('m')" :class="active_tab === 'm' ? 'is-active' : ''"><a>Monthly</a></li>
            <li @click="setTab('c')" :class="active_tab === 'c' ? 'is-active' : ''"><a>Consumption profile</a></li>
          </ul>
        </div>
      </div>


      <div class="container" v-if="!empty">
        <div class="bar-chart">
          <client-only>
            <DailyChart v-if="active_tab == 'd'" :supply="supply"/>
            <WeeklyChartByPeriod v-if="active_tab == 'w'" :supply="supply"/>
            <MonthlyChart v-if="active_tab == 'm'" :supply="supply"/>
          </client-only>
        </div>
      </div>

      <div class="container" v-if="empty">
        <hr>
        <article class="message">
          <div class="message-header">
            <p>No data</p>
          </div>
          <div class="message-body">
            There aren't any measure for this supply. Please, try to get some information pressing the following button
              <div class="button is-ight is-outline " @click="readMeasures"
                    v-bind:class="{ 'is-loading': loading }">
                <span><FontAwesomeIcon :icon="['fas', 'arrows-rotate']"/> Read measures from e-distribucion</span>
              </div>
          </div>
        </article>

      </div>


    </section>

  </div>

</template>


<script>
import {mapGetters} from 'vuex'
import ExampleChart from "~/components/Chart/ExampleChart";
import WeeklyChartByPeriod from "~/components/Chart/WeeklyChartByPeriod";
import DailyChart from "~/components/Chart/DailyChart";
import MonthlyChart from "~/components/Chart/MonthlyChart";


export default {
  data: () => ({
    supply: {},
    last_day: null,
    active_tab: 'd',
    empty: false,
    loading: false,
  }),
  components: {WeeklyChartByPeriod, DailyChart, ExampleChart, MonthlyChart},
  middleware: ["auth"],
  computed: {},
  methods: {
    setTab: function(status) {
      this.active_tab = status
    },
    readMeasures: function () {
      let contract = this.supply.contracts[0].id;
      let toDate = this.$moment()
      let fromDate = this.$moment().subtract(2,'years');

      this.$axios.$get('/contract/'+contract+'/measure/'+fromDate.format("YYYY-MM-DD")+'/'+toDate.format("YYYY-MM-DD")+'/')
        .then(response => {
          console.log(response.msg)
        });
    },
  },
  mounted: function () {
    this.supply = this.$route.params.supply
    if (this.supply.last_data === null){
      this.empty = true;
    }
    this.last_day = this.$moment(this.supply.last_data).format("DD/MM/YYYY HH:mm")
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
