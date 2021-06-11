<template>
<main class="main">
  <button class="show-popup" @click="togglePopup">Show </button>
  <PopUp :isShow="isShow" @handleClose="closePopup"/>
</main>

</template>

<script>
import PopUp from './components/PopUp.vue';

export default {
  name: 'App',
  components: {
    PopUp
  },
  data(){
    return{
      isShow: false
    };
  },
  methods:{
    togglePopup(){
      this.isShow = !this.isShow;
    },
    closePopup(){
      this.isShow = false;
    }
  }
}
</script>

<style lang="sass">
*
    box-sizing: border-box
    margin: 0
    padding: 0
    font-family:  sans-serif

body
    background: #eee
.main
    width: 100vw 
    height: 100vh 
    display: flex 
    justify-content: center 
    align-items: center
.show-popup
    width: 20%
    height: 40px
    cursor: pointer
    margin: 20 50%
    border: none
    border-radius: 50px
    background: #EA0029
    color: #fff
.popup-wrapper
    position: relative
</style>
