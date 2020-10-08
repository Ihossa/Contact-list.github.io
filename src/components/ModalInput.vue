<template>
  <div class="modal-backdrop" @click.self="closeModal" >
    <div class="modal-backdrop__contant">
      <header class="modal-backdrop__header">
        <h1>Add param</h1>
      </header>
      <form  @submit.prevent="createContact" class="modal-backdrop__form">
        <label>Param title</label>
        <input
          v-model="contactData.Title"
          class="modal-backdrop__contact-data"
          placeholder="Param title"
          required
        >
        <label>Param value</label>
        <input
          v-model="contactData.Value"
          class="modal-backdrop__contact-data"
          placeholder="Param value"
          required
        >
        <div class="modal-backdrop__button">
          <button @submit = "createContact" class="modal-backdrop__button-create button">add</button>
          <button @click = "closeModal" class="modal-backdrop__button-cancel button">cancel</button>
        </div>
      </form>
    </div>
  </div>
</template>
<script>
export default {
  name: 'modal',
  data: () => {
    return {
      contactData: {
        id: Date.now(),
        Value: '',
        Title: ''
      }
    }
  },
  methods: {
    closeModal () {
      this.$emit('close-modal')
    },
    createContact (el) {
      this.$emit('add-new-param', this.contactData)
      this.closeModal()
    }
  }
}
</script>
<style lang="scss">
  .modal-backdrop {
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
    &__form{
      display: flex;
      flex-direction: column;
    }
    &__contact-data{
      margin: 5px;
      border: none;
      border-bottom: 1px solid gray;
      line-height: 20px;
      outline: none;
    }

    &__button{
      text-align: center;
    }
    &__button-create{
      background: rgb(89, 187, 89)!important;
      width:40%!important;
      &:hover{
        background: rgb(7, 155, 7)!important;
      }
    }
    &__button-cancel{
      background: #d74652!important;
      width:40%!important;
      &:hover{
        background: #ce2230!important;
      }
    }
  }

</style>
