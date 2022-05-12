
<template>
  <div>
    <client-only>
      <Apexchart max-width="300" :options="chartOptions" :series="series" ref="weekly"></Apexchart>
    </client-only>
  </div>
</template>

<script>
export default {
  data: () => ({
    total: 0,
    chartOptions: {
      colors:[
        '#c4dd8c',
        '#96b633',
        '#f2970f',

      ],
      chart: {
        type: 'bar',
        height: 350,
        stacked: true,
        toolbar: {
          show: true
        },
        zoom: {
          enabled: false
        }
      },
      plotOptions: {
        bar: {
          horizontal: false,
        },
      },
      dataLabels: {
        enabled: true
      },
      title: {
        text: 'Last week',
        align: 'center'
      },
      xaxis: {
        type: 'datetime',
      },
      legend: {
        position: 'right',
        offsetY: 40
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
      this.$axios.$get('/supply/4796d3c9-5215-440b-bc96-af00a8136f1c/data/lastWeek/')
        .then(response => {
          console.log(response.msg.data);
          this.$refs.weekly.updateSeries(response.msg.data)
        });
      console.log(this.$refs.weekly);
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
