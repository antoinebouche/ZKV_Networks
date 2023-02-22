<script lang="ts">

// import VueClipboard from 'v-clipboard3'
import Vue from 'vue';

export default {
  // Properties returned from data() become reactive state
  // and will be exposed on `this`.
//   directives: { 'clipboard': VueClipboard.directive },

  data() {
    return {
      res: [],
      ZKValidatorAddress: 'cosmosvaloper1v5y0tg0jllvxf5c3afml8s3awue0ymju89frut'
    }
  },

  // Methods are functions that mutate state and trigger updates.
  // They can be bound as event listeners in templates.
  methods: {
    getData() {
      fetch("https://api.cosmoscan.net/validator/cosmosvaloper1v5y0tg0jllvxf5c3afml8s3awue0ymju89frut")
      .then(res => res.json())
      .then(data => console.log(data))
    }
  },

  // Lifecycle hooks are called at different stages
  // of a component's lifecycle.
  // This function will be called when the component is mounted.
  mounted() {
    this.getData();
  },
  created() {
    // Vue.use(VueClipboard);
  },
}
</script>



<template>

  <html lang="en">
    <header>
      
        ATOM staking rewards<br>
        <span style="color: #9035FF">with purpose*</span>
      
    </header>
    <body>
      <div class="privacy">*Our 10% commission fee helps support ZKP Research and Privacy Tech</div>
      <div class="green-triangle">
        <p class="green-text-1">4<sup style="font-size:18px">th</sup><br></p>
        <p class="green-text-2">biggest validator</p>
      </div>
      <div class="flex-container">
        <div class="purple-rectangle">
          <p class="purple-text">Estimated APR</p>
          <p class="purple-value">19.27%</p>
        </div>
        <div class="purple-rectangle">
          <p class="purple-text">Delegated to us</p>
          <p class="purple-value">9,736,413 
            <span class="token-name fw400">ATOM</span>
          </p>
          <p class="value-usd fw400">144,877,825.44 USD</p>
        </div>
        <div class="purple-rectangle">
          <p class="purple-text">Unbonding period</p>
          <p class="purple-value">21 days</p>
        </div>
      </div>
      <span class="validator">Validator address</span>
      <span class="validator-address">cosmosvaloper1v5y0tg0jllvxf5c3afml8s3awue0ymju89frut</span>
    </body>
  </html>
 
  
 


</template>

<style scoped>

</style>