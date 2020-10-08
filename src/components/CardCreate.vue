<template>
  <div class="modal-backdrop" @click.self="closeModal" >
    <div class="modal-backdrop__contant">
      <header class="modal-backdrop__header">
        <h1>Registration Contact</h1>
      </header>
      <form  @submit.prevent="createContact" class="modal-backdrop__form">
        <label>Name</label>
        <input
          v-model="contactData.Name"
          class="modal-backdrop__contact-data"
          type="text"
          placeholder="Name"
          required
        >
        <label>Phone</label>
        <input
          v-model="contactData.Phone"
          class="modal-backdrop__contact-data"
          type="number"
          @input='/^(\s*)?(\+)?([- _():=+]?\d[- _():=+]?){10,14}(\s*)?$/'
          placeholder="Phone"
          required
        >
        <label>Email</label>
        <input
          v-model="contactData.Mail"
          class="modal-backdrop__contact-data"
          type="email"
          placeholder="Email"
          required
        >
        <div class="modal-backdrop__button">
          <button @submit = "createContact" class="modal-backdrop__button-create button">add contacts</button>
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
      contactsArray: localStorage.getItem('changeList') ? JSON.parse(localStorage.getItem('changeList')) : [],
      contactData: {
        id: String(Date.now()),
        Name: '',
        Phone: '',
        Mail: ''
      }
    }
  },
  methods: {
    closeModal () {
      this.$emit('close-modal')
    },
    createContact (el) {
      this.contactsArray.push(this.contactData)
      localStorage.setItem('changeList', JSON.stringify(this.contactsArray))
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
    &__contact-data{
      margin: 5px;
      border: none;
      border-bottom: 1px solid gray;
      line-height: 20px;
      outline: none;
    }
  }

</style>
