<template>
  <div class="mn">
    <div class="mainContent">
      <transition name='fade'>
      <b-alert class="alrt"
              :show="failedPaymentCutDown"
              dismissible
              variant="danger"
              @dismissed="failedPaymentCutDown=0">
       <p>Транзакция прошла с ошибкой</p>
     </b-alert>
   </transition>
   <transition name='fade'>
     <b-alert class="alrt"
             :show="completePaymentCutDown"
             dismissible
             variant="success"
             @dismissed="completePaymentCutDown=0">
      <p>Ваша транзакция успешно обработана</p>
    </b-alert>
  </transition>
     <b-container fluid>
       <h4>Переводы</h4>
       <hr>
       <div class = "emailInfo">
        Email: {{this.email}}
       </div>
       <div class = "paymentInfo">
         <h4>Информация о трансфере</h4>
       </div>
       <hr>
       <label class="lbl">
        {{this.titleLabel}}
       </label>
        <paymentForm @alr='showAlert'></paymentForm>
       <hr>
     </b-container>
    </div>
 </div>
</template>

<script>

import paymentForm from "./PaymentForm"

export default {
  data(){
    return {
        email: "null",
        titleLabel: 'Ваш номер счета: null',
        completePaymentCutDown: 0,
        failedPaymentCutDown: 0,
    };
  },
  components:{
    paymentForm
  },
  mounted(){
    this.$http.get("http://localhost:9090/bank/getSomeInfo").then(result => {
      if (result.ok) {
        this.email = result.body[0];
        this.titleLabel = 'Ваш номер счета: ' +  result.body[1]
      }
      }, result => {
         console.log('Result ->' + result.status);
      })
  },
  methods: {
    showAlert(value){
      if (value === 'success'){
        this.completePaymentCutDown = 3;
      } else if (value === 'failure'){
        this.failedPaymentCutDown = 3;
      }
    }
  }
}
</script>
<style>
.paymentInfo{
  margin-top: 50px;
}
.mainContent{
  margin-left: 200px;
  margin-right: 200px;
}
.lbl{
  margin-bottom: 20px;
}
</style>
