<template>
  <div>
    <client-only>
      <date-picker
        placeholder="YYYY"
        format="yyyy"
        v-model="date_selected"
        :disabled-dates="disabled_dates"
        minimum-view="year"
        maximum-view="year"
        :monday-first="true"
      />
      <Apexchart max-width="300" :options="chartOptions" :series="series" ref="daily"></Apexchart>
    </client-only>
  </div>
</template>

<script>
export default {
  data: () => ({
    total: 0,
    cups: null,
    date_selected: new Date(),
    disabled_dates: {},
    chartOptions: {
      chart: {
        type: 'bar',
        height: 350,
        toolbar: {
          show: true
        },
        stroke: {
          curve: 'stepline',
        },
        zoom: {
          enabled: false
        }
      },
      dataLabels: {
        enabled: false,
      },
      markers: {
        size: 6,
      },
      plotOptions: {
        bar: {
          horizontal: false,
        },
      },
      title: {
        text: 'Daily',
        align: 'center'
      },
      xaxis: {
        type: 'categories',
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
  props: ['supply'],
  methods: {
    uChart: function () {
      let day = this.$moment(this.date_selected);
      this.$axios.$get('/supply/' + this.supply.id + '/data/months/'+ day.format("YYYY") + '/')
        .then(response => {
          this.$refs.daily.updateSeries(response.msg.data)
        });
    }
  },
  watch: {
    date_selected(val, oldVal) {
      this.uChart()
    },
  },
  mounted: function () {
    let firstDate = this.$moment(this.supply.first_data)
    let lastDate = this.$moment(this.supply.last_data)

    this.date_selected = this.supply.last_data
    this.disabled_dates = {
      to: firstDate.toDate(),
      from: lastDate.toDate()
    },
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
