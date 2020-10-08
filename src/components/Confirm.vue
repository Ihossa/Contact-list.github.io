<template>
  <div class="confirm" @click.self="closeConfirm">
    <div class="confirm__contant">
      <header class="confirm__header">
        <h1>{{header}}</h1>
      </header>
      <div class="confirm__buttons">
          <button class="confirm__button-true button" @click="setCorfirmResult(true)">Yes</button>
          <button class="confirm__button-false button" @click="setCorfirmResult(false)">No</button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'modal',
  data: () => {
    return {
    }
  },
  props: {
    header: { type: String, default: 'Do you want delete it?' }
  },
  methods: {
    setCorfirmResult (res) {
      this.$emit('delete-contact', res)
      this.closeConfirm()
    },
    closeConfirm () {
      this.$emit('close-confirm')
    }
  }
}
</script>
<style lang="scss">
  .confirm {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: center;
    align-items: center;
    &__contant {
      padding: 20px;
      border-radius: 10px;
      background: #FFFFFF;
      box-shadow: 2px 2px 20px 1px;
      overflow-x: auto;
      display: flex;
      flex-direction: column;
    }
    &__header{
      text-align: center;
      font-style: italic;
    }
    &__buttons{
      text-align: center;
    }
    &__button-true{
      width: 30%;
      background: rgb(89, 187, 89)!important;
      &:hover{
        background: rgb(7, 155, 7)!important;
      }
    }
    &__button-false{
      width: 30%;
      background: #d74652!important;
      &:hover {
        background:#ce2230!important;
      }
    }
  }

</style>
