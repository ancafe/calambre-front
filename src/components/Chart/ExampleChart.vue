
<template>
  <div>
    <client-only>
      <Apexchart max-width="300" type="area" :options="chartOptions" :series="series" ref="chart1"></Apexchart>
    </client-only>
  </div>
</template>

<script>
export default {
  data: () => ({
    total: 0,
    chartOptions: {
      chart: {
        height: 350,
        zoom: {
          enabled: false
        }
      },
      stroke: {
        curve: 'straight'
      },
      title: {
        text: 'Vamos!',
        align: 'center'
      },
      dataLabels: {
        enabled: true
      },
      grid: {
        row: {
          colors: ['#f3f3f3', 'transparent'], // takes an array which will be repeated on columns
          opacity: 0.5
        }
      },
      noData: {
        text: 'Loading...'
      }
    },
    series: []
  }),
  methods: {
    uChart: function () {
      this.$axios.$get('http://my-json-server.typicode.com/apexcharts/apexcharts.js/yearly')
        .then(response => {
          this.$refs.chart1.updateSeries([{
            name: 'Sales',
            data: response
          }])
        });
      console.log(this.$refs.chart1);
    }
  },
  mounted: function () {
    this.uChart()
  },
}
</script>
<style scoped>
.daily {
  display: inline-block;
  font-size: 2rem;
  color: #333333;
}
.total {
  display: inline-block;
}
</style>
