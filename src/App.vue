<template>
<main class="main">
  <button v-show="!isShow" class="show-popup" @click="togglePopup">Налоговый вычет</button>
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
    background: #E9413E
.show-popup
    cursor: pointer
    background: none
    border: 1px solid #fff
    box-sizing: border-box
    border-radius: 6px
    color: #fff
    padding: 16px 32px
    font-weight: 500
    font-size: 16px
    line-height: 24px
    transition: all .2s linear
    &:hover
        color: #000
        background: #fff
.popup-wrapper
    position: relative
</style>
