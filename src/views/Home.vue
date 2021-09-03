<template>
  <div class="contacts">
    <h1 class="contacts__title">Contact Book</h1>
    <div v-if="err" class="contacts__error">Some error while loading data</div>
    <div class="contacts__block" v-if="!err">
      <input class="contacts__search" type="text" v-model="searchContact" placeholder="Search contacts">
      <ul class="contacts__list">
        <li class="contacts__item" v-for="(contact, idx) in searchedContacts" :key="contact.id">
<!--          <div class="contacts__ava">-->
<!--            <img :src="contact.avatar" alt="contact picture">-->
<!--          </div>-->

          <div class="contacts__name"><font-awesome-icon class="contacts__edit" icon="pen" @click="editContact()" /><span @click="toggleSlide($event.target)">{{ idx + 1 }} | {{ contact.name }}</span></div>
          <div class="contacts__description">
            <p>username: {{contact.username}}</p>
            <p>phone: {{ contact.phone }}</p>
            <p>email: {{contact.email}}</p>
            <p>address: {{ contact.address.country }}, {{ contact.address.city }},
              {{ contact.address.streetA }}, {{ contact.address.streetB }},
              {{ contact.address.streetC }}, {{ contact.address.streetD }}</p>
            <p class="contacts__favorite"> favorite - <input v-model="contact.favorite" :checked="contact.favorite" type="checkbox"> </p>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import $ from "jquery";

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
    * Filtered array sorted by favorite contacts
    * for displaying contacts that match the input text
    * */
    searchedContacts(){
      return [...this.contacts]
          .sort((a, b) => b.favorite - a.favorite)
          .filter( contact => {
          return (contact.name.toLowerCase().indexOf(this.searchContact.toLowerCase()) !== -1) ||
                 (contact.username.toLowerCase().indexOf(this.searchContact.toLowerCase()) !== -1);
      });
    },
    dataArray(){
      return this.contacts.map((el) => el.name && el.username && el.phone && el.email && el.address && el.favorite);
    }
  },
  mounted(){
    /*
    * Getting initial data, saving to localStorage or throwing an error
    * */
    if(localStorage.getItem('contacts') !== null){
      this.contacts = JSON.parse(localStorage.getItem("contacts") || []);
    }else{
      this.$axios
          .get('https://demo.sibers.com/users')
          .then(response => {
            localStorage.setItem('contacts',JSON.stringify(response.data));
            this.contacts = response.data;
          })
          .catch(error => {
            console.log(error);
            this.err = true;
          });
    }
  },
  methods:{
    /*
    * toggle slide of description of contact by click on contact
    * */
    toggleSlide(ev){
        $(ev).parent(".contacts__name").siblings('.contacts__description').slideToggle();
        $(ev).toggleClass('active');
    },
    editContact(){
      console.log("Редактирование");
    }
  },
  components: {

  },

}
</script>

<style lang="scss" scoped>
/*
* Styles for contacts block
*/
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
    &__edit{
      margin-right: 10px;
    }
    &__name{
      display: flex;
      align-items: center;
      font-size: 18px;
      font-weight: bold;
      cursor: pointer;
      span{
        position: relative;
        &:before,
        &:after{
          content: "";
          position: absolute;
          display: block;
          width: 10px;
          height: 2px;
          background: #eeeeee;
          top: 50%;
          transition: .5s all ease;
        }
        &:before{
          transform: rotate(45deg);
          right: -30px;
        }
        &:after{
          transform: rotate(-45deg);
          right: -36px;
        }
        &.active{
          &:before{
            transform: rotate(-45deg);
          }
          &:after{
            transform: rotate(45deg);
          }
        }
      }
    }
    &__description{
      display: none;
      padding: 10px 0px 0px 5px;
      p{
        margin-bottom: 5px;
      }
    }
  }
</style>