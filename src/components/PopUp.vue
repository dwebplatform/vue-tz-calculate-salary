<template>
  <div class="popup" v-show="isShow">
      <div class="popup__top">
        <div class="popup__title">Налоговый вычет</div>
        <button class="popup__close" @click="closePopUp"><svg width="18" height="18" viewBox="0 0 18 18" fill="none"
            xmlns="http://www.w3.org/2000/svg">
            <path
              d="M11.4226 9.00086L17.4771 2.94467C17.6407 2.78667 17.7712 2.59768 17.8609 2.38872C17.9507 2.17976 17.998 1.95502 17.9999 1.72761C18.0019 1.50019 17.9586 1.27466 17.8725 1.06417C17.7863 0.853686 17.6592 0.662457 17.4984 0.501645C17.3375 0.340833 17.1463 0.213657 16.9358 0.12754C16.7253 0.041423 16.4998 -0.0019115 16.2724 6.46674e-05C16.045 0.00204084 15.8202 0.0492885 15.6113 0.139051C15.4023 0.228813 15.2133 0.359291 15.0553 0.522874L8.99914 6.57735L2.94467 0.522874C2.78667 0.359291 2.59768 0.228813 2.38872 0.139051C2.17976 0.0492885 1.95502 0.00204084 1.72761 6.46674e-05C1.50019 -0.0019115 1.27466 0.041423 1.06417 0.12754C0.853686 0.213657 0.662457 0.340833 0.501645 0.501645C0.340833 0.662457 0.213657 0.853686 0.12754 1.06417C0.041423 1.27466 -0.0019115 1.50019 6.46674e-05 1.72761C0.00204084 1.95502 0.0492885 2.17976 0.139051 2.38872C0.228813 2.59768 0.359291 2.78667 0.522874 2.94467L6.57735 8.99914L0.522874 15.0553C0.359291 15.2133 0.228813 15.4023 0.139051 15.6113C0.0492885 15.8202 0.00204084 16.045 6.46674e-05 16.2724C-0.0019115 16.4998 0.041423 16.7253 0.12754 16.9358C0.213657 17.1463 0.340833 17.3375 0.501645 17.4984C0.662457 17.6592 0.853686 17.7863 1.06417 17.8725C1.27466 17.9586 1.50019 18.0019 1.72761 17.9999C1.95502 17.998 2.17976 17.9507 2.38872 17.8609C2.59768 17.7712 2.78667 17.6407 2.94467 17.4771L8.99914 11.4226L15.0553 17.4771C15.2133 17.6407 15.4023 17.7712 15.6113 17.8609C15.8202 17.9507 16.045 17.998 16.2724 17.9999C16.4998 18.0019 16.7253 17.9586 16.9358 17.8725C17.1463 17.7863 17.3375 17.6592 17.4984 17.4984C17.6592 17.3375 17.7863 17.1463 17.8725 16.9358C17.9586 16.7253 18.0019 16.4998 17.9999 16.2724C17.998 16.045 17.9507 15.8202 17.8609 15.6113C17.7712 15.4023 17.6407 15.2133 17.4771 15.0553L11.4226 8.99914V9.00086Z"
              fill="#EA0029" />
          </svg></button>
      </div>
      <div class="popup__text">
        Используйте налоговый вычет чтобы погасить ипотеку досрочно. Размер налогового вычета составляет
        не&nbsp;более&nbsp;13%&nbsp;от
        своего
        официального годового дохода.
      </div>
      <div class="popup__payment">
        <div class="popup__payment-title">Ваша зарплата в месяц</div>
        <div class="popup__payment-input-wrapper">
          <input class="popup__payment-input" v-model="salary" placeholder="Введите данные" />
        </div>
        <div class="error">{{error}}</div>
        <button class="popup__payment-calculate-btn" @click="handleCalculate">Рассчитать</button>
        <div class="popup__total">
          <div class="popup__total-title">Итого можете внести в качестве досрочных:</div>
          <div v-if="calculatedTable.length>0" class="popup__total-prices">
          <div v-for="calculatedTableEl in calculatedTable" :key="calculatedTableEl.index"  class="popup__total-price">
              <label class="popup__total-price-checkbox">
                <input class="popup__total-price-checkbox-input" type="checkbox" v-model="calculatedTableEl.checked" /> 
                <CheckBox v-if="calculatedTableEl.checked"/>
              </label>
              <div class="popup__total-price-wrapper">
                <div class="popup__total-price">
                  <div class="popup__total-price-val">{{calculatedTableEl.price}}</div>
                  <div class="popup__total-price-year">в {{calculatedTableEl.index}}ый год</div>
                </div>
              </div>
            </div>
            
          </div>
          <div class="popup__payment-choose-btns">
            <div class="popup__payment-choose-controls-wrapper">
              <div class="popup__payment-choose-question">Что уменьшаем ?</div>
              <div class="popup__payment-choose-controls">
                <button class="popup__payment-choose-btn popup__payment-choose-btn--active">Платеж</button>
                <button class="popup__payment-choose-btn">Срок</button>
              </div>
            </div>
          </div>
        </div>
        <button class="popup__add-btn">Добавить</button>
      </div>
      </div>
</template>
<script>
import CheckBox from '../components/ui/CheckBox.vue';
export default {
  props:{
    isShow: Boolean,
    handleClose: Function
  },
  components:{
    CheckBox
  },
  data(){
    return {
      salary:'',
      error:'',
      calculatedTable:[]
    };
  },
  methods:{
    handleCalculate(){
      if(isNaN(this.salary)){
        this.error = 'Вы ввели не валидное число';
        return;
      }
      let priceForYear = ( parseInt(this.salary)*12)*0.13;

      let amountOfYears = Math.floor(260_000/priceForYear); 
      let remaindPrice = 260_000 % priceForYear;
      // создаем массив из кнопок
      const calculatedRes = [];
      let totalYears = remaindPrice>0?amountOfYears+1:amountOfYears;
      for(let i=0;i<totalYears;i++){
        if(i===totalYears-1){
        calculatedRes.push({
                  checked: false,
                  index:i+1,
                  price:remaindPrice
                });
        } else {
        calculatedRes.push({
                  checked: false,
                  index:i+1,
                  price:priceForYear
                })
        }
      } 
      this.calculatedTable =calculatedRes
    },
    closePopUp(){
        this.$emit('handleClose');
    }
  },
  
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
.center
    width: 100%
    height: 100vh
    display: flex
    justify-content: center
    align-items: center
.error
    color: red
    font-size: 14px
.popup
    max-width: 453px
    background-color: #fff
    padding: 32px
    border-radius: 30px
    position: absolute
    top: 2vh
    left: 50%
    transform: translate(-50%,0)
    &.showed
        display: block
    &__top
        display: flex
        justify-content: space-between
        margin-bottom: 1rem
    &__title
        font-weight: 500
        font-size: 28px
    &__close
        cursor: pointer
        border: none
        background: transparent
        position: relative
        top: -9px
    &__text
        font-weight: 400
        font-size: 14px
        color: #808080
        margin-bottom: 24px
        line-height: 24px
    &__payment-title
        font-weight: 500
        font-size: 14px
        margin-bottom: 1rem
    &__payment-input-wrapper
        display: flex
        margin-bottom: 8px
    &__payment-input
        flex: 1
        padding: 8px 10px
    &__payment-calculate-btn
        border: none
        background: transparent
        color: #EA0029
        font-weight: 500
        cursor: pointer
        margin-bottom: 24px
    &__payment-choose-controls-wrapper
        display: flex
        align-items: center
        justify-content: space-between
        margin-right: 55px
    &__payment-choose-question
        font-weight: 500
        font-size: 14px
    &__payment-choose-controls
        display: flex
        gap: 1rem
    &__payment-choose-btn
        padding: 6px 12px
        border-radius: 50px
        border: none
        cursor: pointer
        &--active
            color: #fff
            background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56
    &__add-btn
        width: 100%
        color: #fff
        padding: 16px
        border: none
        cursor: pointer
        font-weight: 500
        background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56
        box-shadow: 0px 0px 24px rgba(234, 0, 41, 0.33)
        border-radius: 6px
    &__total
        margin-bottom: 2.5rem    
    &__total-price-checkbox
        cursor: pointer
        border: 1px solid #DFE3E6
        width: 22px 
        height: 20px
        margin-right: 11px
        border-radius: 6px
        display: flex
        svg
            position: relative
            top: -1px
            left: 0px
            flex: 1
            width: 20px
    &__total-price-checkbox-input
        position: absolute
        left: -9999px
    &__total-title
        font-size: 14px
        margin-bottom: 1rem
    &__total-prices
        width: 100% 
    &__total-price
        width: 100%
        position: relative
        &::after
            content: ''
            display: block
            width: 100%
            height: 2px
            top: 30px
            position: absolute
            background: #eeee  
    &__total-price-wrapper
        width: 100%
  
    &__total-price
        display: flex
        font-size: 14px
        margin-bottom: 1rem
    &__total-price-val
        margin-right: 3px
    &__total-price-year
        color: rgb(128, 128, 128)    
@media screen and ( max-width: 750px)
        .popup
          
            padding: 20px
            width: 98vw
            &.popup__title

                font-size: 22px
            &__close
                top: 0
            &__payment-choose-btns
                width: 100%
                margin-right: auto
            &__payment-choose-question
                margin-bottom: 1rem
            &__payment-choose-controls-wrapper
              display: block
            &__payment-input-wrapper
                margin-top: 22px

</style>