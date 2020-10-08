<template>
  <div>
    <div class="contact-list" v-if="!isShowContactCard">
      <header class="contact-list__header">
        <h1>Contact List</h1>
      </header>
      <button class="contact-list__create-button button" @click="isShowCardCreate = true">+ add new contact</button>
      <div v-for="contactCard in contactList" :key="contactCard.id" class="contact-list__contact">
        <div class = 'contact-list__contactParametrs'>
          <span class = 'contact-list__contactParametr'>{{contactCard.Name}}</span>
          <span class = 'contact-list__contactParametr'>{{contactCard.Phone}}</span>
          <span class = 'contact-list__contactParametr'>{{contactCard.Mail}}</span>
        </div>
        <div class="contact-list__block-button">
          <button
            class="contact-list__button-edit button"
            @click="showContactCard(contactCard.id)"
            >edit</button>
          <button
            class="contact-list__button-delete button"
            @click="showConfirm(contactCard.id)"
            >delete</button>
        </div>
      </div>
    </div>
    <ContactCard
      :contactId = 'currentId'
      v-if="isShowContactCard"
      @show-contact-list = "showContactList"/>
    <CardCreate
      @close-modal = "closeModal"
      v-if="isShowCardCreate"
    />
    <Confirm
      v-if="isShowConfirm"
      @close-confirm = "isShowConfirm = false"
      @delete-contact = "deleteContact"/>
  </div>
</template>

<script>
import ContactCard from '@/views/ContactCard'
import CardCreate from '@/components/CardCreate'
import Confirm from '@/components/Confirm'

export default {
  name: 'ContactList',
  components: {
    ContactCard,
    CardCreate,
    Confirm
  },
  data: () => {
    return {
      isShowContactCard: false,
      isShowCardCreate: false,
      isShowConfirm: false,
      currentId: null,
      contactList: localStorage.getItem('changeList') ? JSON.parse(localStorage.getItem('changeList')) : []
    }
  },
  methods: {
    closeModal () {
      this.isShowCardCreate = false
    },
    showContactList () {
      this.isShowContactCard = false
    },
    deleteContact (res) {
      if (res) {
        const currentContactId = this.contactList.findIndex(currentContact => currentContact.id === this.currentId)
        this.contactList.splice(currentContactId, 1)
        localStorage.setItem('changeList', JSON.stringify(this.contactList))
      }
    },
    showConfirm (id) {
      this.currentId = id
      this.isShowConfirm = true
    },
    showContactCard (id) {
      this.currentId = id
      this.isShowContactCard = true
    }
  },
  beforeUpdate () {
    this.contactList = localStorage.getItem('changeList') ? JSON.parse(localStorage.getItem('changeList')) : []
  }
}
</script>

<style lang="scss">
  .button{
    font-family: inherit;
    appearance: none;
    border: 0;
    margin: 2px;
    border-radius: 5px;
    background: #4676d7;
    color: #fff;
    font-size: 1rem;
    cursor: pointer;
    &:hover {
      background: #1d49aa;
    }
    &:focus {
      outline: none;
      box-shadow: 0 0 0 4px #cbd6ee;
    }
  }
  .contact-list {
    margin: 0 14.5%;
    &__header{
      font-style: italic;
      text-align: center;
    }
    &__create-button{
      display: block;
      min-width: 100px;
      padding: 8px 16px;
      font-size: 1rem;
    }
    &__contact{
      background: rgba(188, 199, 187, 0.671);
      border-radius: 5px;
      box-shadow: 0 0 10px rgba(0,0,0,0.3);
      display: inline-block;
      justify-content: space-between;
      margin: 15px;
      padding: 5px;
    }
    &__contactParametrs{
      width: 50%;
      display: flex;
      flex-direction: column;
      flex-grow: 21;
      min-width: 200px;
    }
    &__button-edit{
      font-family: inherit;
      appearance: none;
      border: 0;
      border-radius: 5px;
      background: #4676d7;
      color: #fff;
      font-size: 1rem;
      cursor: pointer;

    }
    &__button-delete{
      background: #d74652;
      &:hover {
        background:#ce2230;
      }
    }

  }
</style>
