<template>

    <html lang="en">
      <header>
          EVMOS staking rewards<br>
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
            <p v-if="validatorInfo.tokens" class="purple-value">{{ validatorInfo.tokens.toLocaleString() }}
              <span class="token-name fw400">EVMOS</span>
            </p>
            <p class="value-usd fw400">144,877,825.44 USD</p>
          </div>
          <div class="purple-rectangle">
            <p class="purple-text">Unbonding period</p>
            <p class="purple-value">21 days</p>
          </div>
        </div>
  
        <span class="validator">Validator address</span>
        <span class="validator-address">{{EvmosAddress}}
          <img class="copy-button" src="src/assets/images/copy.svg" @click="copyText">
        </span>
  
        <div class="flex-container">
          <div class="grey-rectangle">
              <img class="grey-img vote" src="src/assets/images/vote1.svg">
              <p class="grey-text">Voting Power</p>
              <p class="grey-value">4.60%</p>
          </div>
          <div class="grey-rectangle">
              <img class="grey-img people" src="src/assets/images/people.svg">
              <p class="grey-text">Delegators</p>
              <p class="grey-value">2,723</p>
          </div>
          <div class="grey-rectangle">
              <img class="grey-img fee" src="src/assets/images/dollarcoin.svg">
              <p class="grey-text">Commission</p>
              <p class="grey-value">{{commissionRate}}</p>
          </div>
        </div>
        <div class="delegate-button">
          Delegate To Us
        </div>
        <div>
          <h1>Validator Information</h1>
          <ul>
          <li>Stake: {{ validatorInfo.tokens }}</li>
          <li>Expected Blocks: {{ validatorInfo?.num_expected_blocks }}</li>
          <li>Expected Chunks: {{ validatorInfo?.num_expected_chunks }}</li>
          <li>Produced Blocks: {{ validatorInfo?.num_produced_blocks }}</li>
          <li>Produced Chunks: {{ validatorInfo?.num_produced_chunks }}</li>
          <li>Validator Stake Struct Version: {{ validatorInfo?.validator_stake_struct_version }}</li>
          </ul>
       </div>
  
        
      </body>
    </html>
   
</template>  



<script lang="ts">

import Vue from 'vue';
import axios from 'axios';
import type { AxiosResponse } from 'axios';



interface ZKValidatorEvmosData {
    operator_address: string;
    tokens: number;
    commission: {
    commission_rates: {
      rate: string;
    };
  };
}



export default {
    // Properties returned from data() become reactive state
    // and will be exposed on `this`.
    //   directives: { 'clipboard': VueClipboard.directive },
    data() {
        return {
            res: [],
            EvmosAddress: "evmosvaloper1hdslyd0vwj8ym368tqc5jdpu6hxrqvx9zex5yz" as string,
            validatorInfo: {} as ZKValidatorEvmosData,
        };
    },
    // Methods are functions that mutate state and trigger updates.
    // They can be bound as event listeners in templates.
    methods: {
        copyText() {
            navigator.clipboard.writeText(this.EvmosAddress);
        },

        async fetchEvmosData(): Promise<void> {
            try {
                const response: AxiosResponse<{ validator: ZKValidatorEvmosData }> = await axios.get('https://evmos-api.polkachu.com/cosmos/staking/v1beta1/validators/evmosvaloper1hdslyd0vwj8ym368tqc5jdpu6hxrqvx9zex5yz');
                this.validatorInfo = response.data.validator;
                this.validatorInfo.tokens = Math.floor(Number(this.validatorInfo.tokens) / 10**18);
                const commissionRate = parseFloat(this.validatorInfo.commission.commission_rates.rate);
                const commissionPercentage = commissionRate * 100;
                console.log(this.validatorInfo);
            } 
            catch (error) {
                console.error(error);
            }
        },
        async fetchData() {
            const validator = await this.fetchEvmosData();
            console.log(validator);
            // Do something with the validator data here
        },
    },
    
    mounted() {
        this.fetchEvmosData();
        console.log(this.validatorInfo);
    },
    async created() {
    },
}

</script>

<style>

</style>