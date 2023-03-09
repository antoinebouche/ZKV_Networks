<template>

    <html lang="en">
      <header>
          EVMOS staking rewards<br>
          <span style="color: #9035FF">with purpose*</span>
      </header>
      <body>
        <div class="privacy">*Our 10% commission fee helps support ZKP Research and Privacy Tech</div>
        <div class="green-triangle">
          <!-- <p class="green-text-1">4<sup style="font-size:18px">th</sup><br></p> -->
          <p class="green-text-3">Since</p>
          <p class="green-text-4">Block #1</p>
          <!-- <p class="green-text-2">biggest validator</p> -->
        </div>
        <div class="flex-container">
          <div class="purple-rectangle">
            <p class="purple-text">Estimated APR</p>
            <p class="purple-value">87.33%</p>
          </div>
          <div class="purple-rectangle">
            <p class="purple-text">Delegated to us</p>
            <p v-if="validatorInfo.tokens" class="purple-value">{{ validatorInfo.tokens.toLocaleString() }}
              <span class="token-name fw400">EVMOS</span>
            </p>
            <p v-if="validatorInfo.valueUSD" class="value-usd fw400">{{ validatorInfo.valueUSD.toLocaleString() }} USD</p>
          </div>
          <div class="purple-rectangle">
            <p class="purple-text">Unbonding period</p>
            <p class="purple-value">21 days</p>
          </div>
        </div>
        <div class="address-div">
          <span class="validator">Validator address</span>
          <span class="validator-address">{{EvmosAddress}}
            <img class="copy-button" src="src/assets/images/copy.svg" @click="copyText">
            <div v-if="showCopiedMessage" class="copied">Copied!</div>
          </span>
        </div>
        <div class="flex-container">
          <div class="grey-rectangle">
              <img class="grey-img vote" src="src/assets/images/vote1.svg">
              <p class="grey-text">Voting Power</p>
              <p class="grey-value">{{(validatorInfo.tokens/validatorInfo.totalBondedTokens*100).toFixed(2)}}%</p>
          </div>
          <div class="grey-rectangle">
              <img class="grey-img people" src="src/assets/images/people.svg">
              <p class="grey-text">Delegators</p>
              <p class="grey-value">{{ validatorInfo.totalDelegators }}</p>
          </div>
          <div class="grey-rectangle">
              <img class="grey-img fee" src="src/assets/images/dollarcoin.svg">
              <p class="grey-text">Commission</p>
              <p v-if="validatorInfo.commission" class="grey-value">{{parseFloat(validatorInfo.commission.commission_rates.rate)*100}}%</p>
          </div>
        </div>
        <a href="https://wallet.keplr.app/chains/evmos?modal=validator&chain=evmos_9001-2&validator_address=evmosvaloper1hdslyd0vwj8ym368tqc5jdpu6hxrqvx9zex5yz" target="_blank">
          <div class="delegate-button">
            Delegate To Us
          </div>
        </a>
        <TheNetworkInfo :networkVersion="networkVersion.version" :nodeInfo="nodeInfo.network" />
        <TheGovernanceVotes :allProposals="allProposals"/>
        <div class="info-section">
          <div class="useful-links">Useful links</div>
          <div>
            <a href="https://www.mintscan.io/evmos" target="_blank">
              <div class="link">
                Block explorer
              </div>
            </a>
            <a href="https://www.mintscan.io/evmos/validators/evmosvaloper1hdslyd0vwj8ym368tqc5jdpu6hxrqvx9zex5yz" target="_blank">
              <div class="link">
                More info about our validator node
              </div>
            </a>
            <a href="https://evmos-rpc.polkachu.com/" target="_blank">
              <div class="link">
                RPC endpoint
              </div>
            </a>
            <TheUptimeTable :uptime="99" :latency="5"/>
            
          </div>
        </div>
      </body>
    </html>
   
</template>  



<script lang="ts">

import Vue from 'vue';
import axios from 'axios';
import type { AxiosResponse } from 'axios';
import TheNetworkInfo from '/Users/antoine/Documents/ZKValidator/NetworkDashboard/src/components/TheNetworkInfo.vue'
import TheGovernanceVotes from '/Users/antoine/Documents/ZKValidator/NetworkDashboard/src/components/TheGovernanceVotes.vue'
import TheUptimeTable from '/Users/antoine/Documents/ZKValidator/NetworkDashboard/src/components/TheUptimeTable.vue'


interface ZKValidatorEvmosData {
    operator_address: string;
    tokens: number;
    valueUSD: number;
    totalBondedTokens: number;
    totalDelegators: number;
    pagination: {
      total: number;
    }
    commission: {
      commission_rates: {
        rate: string;
      };
    };
}

interface Ticker {
  converted_last: {
    usd: number;
  };
}

interface Pool {
    not_bonded_tokens: number;
    bonded_tokens: number;
}

interface Delegators {
  total: number;
}

interface node_info {
  network: string;
};

interface network_version {
    version: string;
};

export default {

  name: 'Evmos',
    // Properties returned from data() become reactive state
    // and will be exposed on `this`
    data() {
        return {
            res: [],
            EvmosAddress: "evmosvaloper1hdslyd0vwj8ym368tqc5jdpu6hxrqvx9zex5yz" as string,
            validatorInfo: {} as ZKValidatorEvmosData,
            priceData: {} as Ticker,
            showCopiedMessage: false as boolean,
            poolTokens: {} as Pool,
            allDelegators: {} as ZKValidatorEvmosData,
            nodeInfo: {} as node_info,
            networkVersion: {} as network_version,
            APIurl: 'https://evmos-api.polkachu.com/cosmos/gov/v1beta1/proposals' as string,
            allProposals: [] as string[],
            url: '' as string || null,
        };
    },

    components: {
    TheNetworkInfo,
    TheGovernanceVotes,
    TheUptimeTable,
    },

    methods: {
        copyText() {
            navigator.clipboard.writeText(this.EvmosAddress);
            this.showCopiedMessage = true;
            setTimeout(() => {
              this.showCopiedMessage = false;
            }, 3000); // Hide message after 3 seconds
        },

        async fetchEvmosData(): Promise<void> {
            try {
                const response: AxiosResponse<{ validator: ZKValidatorEvmosData }> = await axios.get('https://evmos-api.polkachu.com/cosmos/staking/v1beta1/validators/evmosvaloper1hdslyd0vwj8ym368tqc5jdpu6hxrqvx9zex5yz');
                this.validatorInfo = response.data.validator;
                this.validatorInfo.tokens = Math.floor(this.validatorInfo.tokens/ 10**18);

                const response2: AxiosResponse<{ tickers: Ticker[] }> = await axios.get(
      "https://api.coingecko.com/api/v3/coins/evmos?localization=false&tickers=true&market_data=false&community_data=false&developer_data=false&sparkline=false"); 
                this.priceData = response2.data.tickers[0];
                this.validatorInfo.valueUSD = Math.floor((this.validatorInfo.tokens)*this.priceData.converted_last.usd);

                const response3: AxiosResponse<{pool: Pool}> = await axios.get("https://evmos-api.polkachu.com/cosmos/staking/v1beta1/pool");
                this.poolTokens = response3.data.pool;
                this.validatorInfo.totalBondedTokens = Math.floor(this.poolTokens.bonded_tokens/ 10**18);

                const response4: AxiosResponse<{pagination: Delegators}> = await axios.get("https://evmos-api.polkachu.com/cosmos/staking/v1beta1/validators/evmosvaloper1hdslyd0vwj8ym368tqc5jdpu6hxrqvx9zex5yz/delegations");
                this.validatorInfo.totalDelegators = response4.data.pagination.total;
            } 
            catch (error) {
                console.error('error');
            }
        },

        async fetchNetworkInfo(): Promise<void> {
            try{
                const response: AxiosResponse<any> = await axios.get('https://evmos-api.polkachu.com/cosmos/base/tendermint/v1beta1/node_info');
                this.nodeInfo.network = response.data.default_node_info.network;
                this.networkVersion.version = response.data.application_version.version;
            }
            catch (error) {
                console.error('error');
            }
        },

        async fetchVoteData(): Promise<void> {

          try {

            this.url = 'https://evmos-api.polkachu.com/cosmos/gov/v1beta1/proposals';
            this.allProposals;

            while (this.url) {
                const response: AxiosResponse = await axios.get(this.url);
                const { proposals, pagination } = response.data;
                this.allProposals.push(...proposals);

                this.url = pagination.next_key ? `${this.url}?pagination.key=${pagination.next_key}` : null;
            }
            
          } catch (error) {
                console.log('error');
          }

          this.allProposals.reverse();
        },
    },
    
    mounted() {
        this.fetchEvmosData();
        this.fetchNetworkInfo();
        this.fetchVoteData();
    },
}

</script>

<style>

.info-section{
  margin: auto;
  width: 649px;
  border: solid 1px #464646;
  text-align: left;
  padding-left: 8px;
  
}

.useful-links{
  height: 33px;
  font-size: 16px;
}

.link{
  font-size: 14px;
  color: #79A1FF;
}

</style>