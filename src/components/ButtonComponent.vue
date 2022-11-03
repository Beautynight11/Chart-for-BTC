<template>
  <button
      class="button"
      @click="clickFunction(data)"
  >
    {{name}}
  </button>
</template>

<script>
export default {
  name: "ButtonComponent",
  props: {
    clickFunction: {
      required: true,
      type: Function,
    },
    data: {
      required: true,
      type: String
    },
    name: {
      required: true,
      type: String
    }
  }
}
</script>

<style scoped lang="sass">
.button
  opacity: .3
  cursor: pointer
  border: 1px solid #000
  background-color: #FFF
  padding: 5px 10px
  margin-bottom: 10px

  &:hover
    opacity: 1
    color: #FFF
    border: 1px solid #000
    background-color: #40206c

</style>