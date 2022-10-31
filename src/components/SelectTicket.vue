<template>
  <div class="selectTicket">
      <div
          class="selectTicket__input"
          @click="toggleIsShow"
      >
        {{this.name}}
      </div>
    <div
        class="selectTicket__list"
        v-if="isShow"
    >
      <div
          v-for="item in info"
          :key="item.id"
          @click="getElement(item.code); this.isShow = false;"
          class="selectTicket__item"
      >
        {{item.code}}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SelectTicket",
  props: {
    info: {
      required: false,
      type: Object,
    },
    getElement: {
      required: true,
      type: Function
    },
  },
  data() {
    return {
      isShow: false,
      name: 'Select...',
      code: null,
    }
  },
  methods: {
    toggleIsShow() {
      this.isShow = !this.isShow;
      this.name = ''
    }

  }
}
</script>

<style scoped lang="sass">
.selectTicket
  margin-bottom: 50px

  &__input
    background-color: #fff
    border: 1px solid #40206c
    margin-top: 50px
    width: 100%
    height: 35px
    padding: 5px
    cursor: pointer
    outline: none

  &__list
    background-color: #fff
    border: 1px solid #40206c
    margin-top: 10px

  &__item
    border-bottom: 1px solid #40206c
    padding: 5px
    cursor: pointer

    &:last-child
      border-bottom: none

    &:hover
      background-color: #40206c
      color: white
      opacity: .3



</style>