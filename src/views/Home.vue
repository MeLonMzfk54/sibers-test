<template>
  <div class="contacts">
    <h1 class="contacts__title">Contact Book</h1>
    <div v-if="err" class="contacts__error">Some error while loading data</div>
    <div class="contacts__block" v-if="!err">
      <input class="contacts__search" type="text" v-model="searchContact" placeholder="Search contacts">
      <ul class="contacts__list">
        <li class="contacts__item" v-for="(contact, idx) in searchedContacts" :key="idx">
          {{ contact }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data: function (){
    return{
      contacts: [],
      err: false,
      searchContact: '',
    }
  },
  computed: {
    /*
    * Filtered array for displaying contacts that match the input text
    * */
    searchedContacts(){
      return this.contacts.filter( contact => {
          return (contact.name.toLowerCase().indexOf(this.searchContact.toLowerCase()) !== -1) ||
                 (contact.username.toLowerCase().indexOf(this.searchContact.toLowerCase()) !== -1);
      });
    }
  },
  mounted(){
    /*
    * Getting initial data or throwing an error
    * */
    this.$axios
        .get('https://demo.sibers.com/users')
        .then(response => this.contacts = response.data)
        .catch(error => {
          console.log(error);
          this.err = true;
        });
  },
  components: {

  },

}
</script>

<style lang="scss" scoped>
  .contacts{
    &__error{
      background: rgba(255, 0, 0, 0.2);
      border-radius: 10px;
      color: red;
      display: flex;
      justify-content: flex-start;
      padding: 10px 5px;
      align-items: center;
      font-size: 30px;
    }
    &__search{
      border-bottom: 1px solid #eee;
      padding-bottom: 5px;
      margin: 20px 0px;
      background: transparent;
      font-size: 15px;
      color: white;
      &:hover,
      &:focus{
        border-color: aquamarine;
        color: aquamarine;
      }
      &::placeholder{
        color: white;
        font-size: 15px;
      }
      &::-moz-placeholder{
        color: white;
        font-size: 15px;
      }
      &::-ms-input-placeholder{
        color: white;
        font-size: 15px;
      }
      &::-webkit-input-placeholder{
        color: white;
        font-size: 15px;
      }
    }
    &__title{
      text-align: center;
      margin-bottom: 50px;
    }
    &__list{
      display: flex;
      flex-direction: column;
      gap: 20px;
    }
    &__item{}
  }
</style>