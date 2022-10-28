<template>
  <div class="app">
    <div class="container" >
      <SelectTicket
          :info="this.info"
          :get-element='getElement'
      />
      <div class="app__price">
        <PriceData
            v-for="(item, index) in this.data"
            :key="index"
            :data="item"
            :delete-data="deleteData"
        />
      </div>
      <div class="app__chart">
        <LineChart/>
      </div>
    </div>
  </div>
</template>

<script>
import PriceData from "@/components/PriceData";
import SelectTicket from "@/components/SelectTicket";
import LineChart from "@/components/LineChart";

export default {
  name: 'App',
  components: { LineChart, SelectTicket, PriceData },
  data() {
    return {
      info: null,
      errored: false,
      errorMessage: 'Error',
      interval: '',
      data: [],
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
      }, 6000)
    },
    getElement(el) {
      if (Object.keys(this.info).includes(el)
          && !this.data.includes(this.info[el])
      ) {
        this.data.push({
          code: el,
          rate: this.info[el].rate
        })
      }
    },
    deleteData(id) {
      this.data = this.data.filter((data) => data.code !== id);
    }
  },
  mounted () {
    this.getCounter();
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

.container
  margin-left: auto
  margin-right: auto
  padding-left: 15px
  padding-right: 15px
  max-width: 1140px

</style>
