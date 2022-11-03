<template>
  <div class="ticketData">
    <div class="ticketData__box">
      <div class="ticketData__name">BTC to {{this.data.code}}</div>
      <div class="ticketData__rate">{{this.data.rate}}</div>
      <div class="ticketData__btns">
        <ButtonComponent
            :data="this.data.code"
            :click-function="getChartData"
            name="Show chart"
        />
        <ButtonComponent
            :data="this.data.code"
            :click-function="deleteData"
            name="Delete"
        />
      </div>
    </div>
  </div>
</template>

<script>
import ButtonComponent from "@/components/ButtonComponent";
export default {
  name: "TicketData",
  components: {ButtonComponent},
  props: {
    data: {
      required: true,
      type: Object,
    },
    deleteData: {
      type: Function,
      required: true
    },
    getChartData: {
      required: true,
      type: Function
    },
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
    text-align: center

  &__name
    margin-top: 30px
    margin-bottom: 20px
    opacity: .3
    font-size: 14px

  &__rate
    font-size: 30px
    margin-bottom: 30px

  &__btns
    display: flex
    flex-direction: column
    flex: 1 0 100px
    width: 100px
    margin-right: auto
    margin-left: auto

</style>