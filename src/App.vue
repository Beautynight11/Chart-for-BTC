<template>
  <div class="app">
    <div class="container" >
      <SelectTicket
          :info="this.info"
          :get-element='getElement'
      />
      <div class="app__price">
        <TicketData
            v-for="(item, index) in this.data"
            :key="index"
            :data="item"
            :delete-data="deleteData"
            :get-chart-data="getData"
        />
      </div>
      <div class="app__chart">
        <LineChart
            :chart-data="this.chartData"
        />
      </div>
    </div>
  </div>
</template>

<script>
import TicketData from "@/components/TicketData";
import SelectTicket from "@/components/SelectTicket";
import LineChart from "@/components/LineChart";

export default {
  name: 'App',
  components: { LineChart, SelectTicket, TicketData },
  data() {
    return {
      info: null,
      errored: false,
      errorMessage: 'Error',
      interval: '',
      time: null,
      data: [],
      labels: [],
      chartData: {
        labels: [],
        datasets: [],
      },
      chartName: null
    }
  },
  methods: {
    async getCounter () {
      const axios = require('axios')
      await axios
          .get('https://api.coindesk.com/v1/bpi/currentprice.json')
          .then(value => {
            this.info = value.data.bpi;
          })
          .catch(error => {
            this.errorMessage = error.message;
            this.errored = true;
          })
    },
    stopTimer () {
      if (this.interval) {
        window.clearInterval(this.interval);
      }
    },
    startTimer () {
      this.stopTimer();
      this.interval = window.setInterval(() => {
        this.getCounter();
        this.getTime();
        this.updateTicket();
      }, 10000)
    },
    getElement(el) {
      if (Object.keys(this.info).includes(el)
          && !this.labels.includes(this.info[el].code))
      {
        this.labels.push(el);
        this.data.push({
          code: el,
          rate: this.info[el].rate_float
        })
      }
    },
    deleteData(id) {
      this.data = this.data.filter(data => data.code !== id);
      this.labels = this.labels.filter(data => data !== id);
      this.chartData =  {
        labels: [],
        datasets: [],
      }
    },
    updateTicket () {
      this.data.forEach(ticket => {
        ticket.rate = this.info[ticket.code].rate_float;
      });
      this.chartData.labels.push(this.time);
      this.chartData.datasets.forEach(data =>
          data.data.push(this.info[data.label].rate_float)
      )
    },
    getTime() {
      this.time = new Date().toLocaleTimeString();
    },
    getData(el) {
      this.chartData.datasets = [{
        label: el,
        backgroundColor: '#40206c',
        data: [this.info[el].rate_float],
        borderColor: '#40206c'
      }];
      this.chartName = el;
      this.chartData.labels = [this.time];
    }
  },
  mounted () {
    this.getTime();
    this.getCounter();
    this.updateTicket();
    this.startTimer();
  },
  beforeUnmount () {
    this.stopTimer();
  },
}
</script>

<style lang="sass">
@font-face
  font-family: 'OpenSans'
  font-style: normal
  font-weight: 300
  src: local('OpenSans-Italic'), url('../fonts/OpenSans-Italic.ttf') format('truetype')

@font-face
  font-family: 'OpenSans'
  font-style: normal
  font-weight: 400
  src: local('OpenSans-Regular'), url('../fonts/OpenSans-Regular.ttf') format('truetype')

@font-face
  font-family: 'OpenSans'
  font-style: normal
  font-weight: 500
  src: local('OpenSans-Medium'), url('../fonts/OpenSans-Medium.ttf') format('truetype')

@font-face
  font-family: 'OpenSans'
  font-style: normal
  font-weight: 600
  src: local('OpenSans-SemiBold'), url('../fonts/OpenSans-SemiBold.ttf') format('truetype')

@font-face
  font-family: 'OpenSans'
  font-style: normal
  font-weight: 700
  src: local('OpenSans-Bold'), url('../fonts/OpenSans-Bold.ttf') format('truetype')


html
  box-sizing: border-box
  position: relative
  -moz-user-select: none
  -ms-user-select: none
  -khtml-user-select: none
  -webkit-user-select: none
  -webkit-touch-callout: none

*, *::before, *::after
  box-sizing: inherit

body
  font-family: 'OpenSans', sans-serif
  font-weight: 400
  margin: 0
  box-sizing: border-box
  color: #40206c
  background-color: #e7dcef
  position: relative
  padding: 0

.app
  &__price
    margin-top: 50px
    display: flex
    justify-content: center
    margin-bottom: 50px

    &__chart
      margin-bottom: 150px

.container
  margin-left: auto
  margin-right: auto
  padding-left: 15px
  padding-right: 15px
  max-width: 1140px

</style>
