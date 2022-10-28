<template>
  <div class="ticketData">
    <div class="ticketData__box">
      <div class="ticketData__name">BTC to {{this.data.code}}</div>
      <div class="ticketData__rate">{{this.data.rate}}</div>
      <button
          class="ticketData__btn"
          @click="deleteData(this.data.code)"
      >
        Delete
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "TicketData",
  props: {
    data: {
      required: true,
      type: Object,
    },
    deleteData: {
      type: Function,
      required: true
    }
  },
    data() {
      return {
      }
    },
}
</script>

<style scoped lang="sass">
.ticketData
  background-color: #fff
  max-width: 200px
  margin-right: 50px

  &:last-child
    margin-right: 0

  &__box
    border: 1px solid #40206c
    width: 200px
    height: 200px
    text-align: center

  &__name
    margin-top: 30px
    margin-bottom: 20px
    opacity: .3
    font-size: 14px

  &__rate
    font-size: 30px
    margin-bottom: 30px

  &__btn
    opacity: .3
    cursor: pointer
    border: 1px solid #000
    background-color: #FFF
    padding: 5px 10px

    &:hover
      opacity: 1
      color: #FFF
      border: 1px solid #000
      background-color: #40206c
</style>