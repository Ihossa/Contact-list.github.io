<template>
  <div class="contact-card">
    <header class="contact-card__header">
      <h1>Contact</h1>
    </header>
    <button
      class="contact-card__back-button button"
      @click="backToContactList"
    >
      <span>back to contact list</span>
    </button>
    <button
      class="button contact-card__cancel"
      @click="returnSave"
      v-if="isVisibleReturnSave"
    >
      <span>cancel saving</span>
    </button>
    <form
      class="contact-card__form"
      @submit.prevent="editContact"
      @keydown="isDisabledBtnChange = false"
      ref = 'form'
    >
      <div
        v-for="(param, index) in contactData"
        :key="index"
        class="contact-card__param"
      >
        <input class="contact-card__key" type="text" v-show = 'index!=="id"' :value="index" disabled>
        <input class="contact-card__value" type="text" :value="param" v-show = 'index!=="id"'>
      </div>
      <div
        v-for="(param, index) in newParams"
        :key="index"
        class="contact-card__new-param"
      >
        <input class="contact-card__key" type="text" :value="param.Title" >
        <input class="contact-card__value" type="text" :value="param.Value">
        <button
          @click.prevent="showConfirm(param.id)"
          class="button contact-card__remote-button">
          <span>remove</span>
        </button>
      </div>
    </form>
    <button
      class="button contact-card__add-param"
      @click="addNewParam"
    >
      <span>add parameter</span>
    </button>
    <div class="contact-card__foot-button">
      <button
        class=" contact-card__save-button"
        @click="editContact"
        :disabled = 'isDisabledBtnChange'
      >
        <span>save change</span>
      </button>
      <button
        class=" contact-card__cancel-button"
        @click="isShowConfirmChange = true"
        :disabled = 'isDisabledBtnChange'
      >
        <span>Cansel change</span>
      </button>
    </div>
    <Confirm
      v-if="isShowConfirm"
      @close-confirm = "isShowConfirm = false"
      @delete-contact = "deleteContact"/>
    <Confirm
      v-if="isShowConfirmChange"
      header = 'Do you want edit it?'
      @close-confirm = "isShowConfirmChange = false"
      @delete-contact = "cancelChange"/>
    <ModalInput
      v-if="isShowModalInput"
      @close-modal = "closeModal"
      @add-new-param = "createParam"/>
  </div>
</template>

<script>
import ModalInput from '@/components/ModalInput'
import Confirm from '@/components/Confirm'

export default {
  name: 'contact-card',
  components: {
    ModalInput,
    Confirm
  },
  data: () => {
    return {
      formData: null,
      isDisabledBtnChange: true,
      isVisibleReturnSave: false,
      isShowModalInput: false,
      isShowConfirm: false,
      isShowConfirmChange: false,
      changesHistory: [],
      contactData: {},
      newParams: [],
      idNewParam: null,
      currentNewParams: null,
      contactList: localStorage.getItem('changeList') ? JSON.parse(localStorage.getItem('changeList')) : []
    }
  },
  props: {
    contactId: { type: String }
  },
  methods: {
    backToContactList () {
      const currentContactId = this.contactList.findIndex(currentContact => currentContact.id === this.contactId)
      this.contactList.splice(currentContactId, 1, this.contactData)
      localStorage.setItem('changeList', JSON.stringify(this.contactList))
      this.$emit('show-contact-list')
    },
    addNewParam () {
      this.isShowModalInput = true
    },
    createParam (newParam) {
      this.isDisabledBtnChange = false
      this.newParams.push(newParam)
    },
    cancelChange (res) {
      if (res) {
        this.editContact()
        this.returnSave()
        this.isDisabledBtnChange = true
      }
    },
    showReturnSave () {
      if (this.changesHistory.length > 1) {
        this.isVisibleReturnSave = true
      } else {
        this.isVisibleReturnSave = false
      }
    },
    returnSave () {
      this.changesHistory.pop()
      this.showReturnSave()
      this.isDisabledBtnChange = true
      this.contactData = this.changesHistory[this.changesHistory.length - 1]
    },
    closeModal () {
      this.isShowModalInput = false
    },
    editContact () {
      const arrInput = []
      const contactObj = {}
      this.$refs.form.forEach((input) => {
        if (input.localName !== 'button') { arrInput.push(input.value) }
      })
      const newArr = arrInput.reduce(function (result, value, index, array) {
        if (index % 2 === 0) { result.push(array.slice(index, index + 2)) }
        return result
      }, [])
      newArr.map((elObj) => {
        contactObj[elObj[0]] = elObj[1]
      })
      this.changesHistory.push(contactObj)
      this.showReturnSave()
      this.contactData = this.changesHistory[this.changesHistory.length - 1]
      this.isDisabledBtnChange = true
      this.newParams = []
    },
    showConfirm (id) {
      this.idNewParam = id
      this.isShowConfirm = true
    },
    deleteContact (res) {
      if (res) {
        const currentContactId = this.contactList.findIndex(currentContact => currentContact.id === this.idNewParam)
        this.newParams.splice(currentContactId, 1)
      }
    }
  },
  created () {
    this.contactList.map((contact) => {
      if (contact.id === this.contactId) {
        this.changesHistory.push(contact)
        this.contactData = this.changesHistory[this.changesHistory.length - 1]
      }
    })
  }
}
</script>
<style lang="scss" scoped>
  .contact-card{
    margin: 0 14.5%;
    &__header{
      font-style: italic;
      text-align: center;
    }
    &__back-button{
      padding: 8px 16px;
      font-size: 1rem;
      margin-bottom: 20px;
    }
    &__form{
      text-align: center;
    }
    &__param{
     text-align: center;
     border-radius: 4px;
    }
    &__new-param{
      transform: translateX(31px);
    }
    &__remote-button{
      line-height: 25px;
      background: #d74652;
      &:hover {
        background:#ce2230;
      }
    }
    &__save-button{
      color:white;
      width: 30%;
      margin: 5px;
      border-radius:5px;
      text-align: center;
      overflow: hidden;
      background: rgb(89, 187, 89);
      cursor: pointer;
      &:focus{
        background: rgb(7, 155, 7);
      }
    }
    &__cancel-button{
      color:white;
      width: 30%;
      margin: 5px;
      border-radius:5px;
      text-align: center;
      overflow: hidden;
      background: #d74652;
      cursor: pointer;
      &:focus {
        background:#ce2230;
      }
    }
    &__add-param{
      width: 10%;
      min-width:100px;
      transform: translateX(10%);
    }
    &__cancel{
      position: absolute;
      right: 20%;
      transform: translateY(15px);
    }
    &__key{
      width: 20%;
      line-height: 25px;
    }
    &__value{
      width: 50%;
      line-height: 25px;
    }
    &__foot-button{
      text-align: center;
    }
  }

</style>
