<template>
    <div class="container">
        <div class="head">Governance votes</div>
        <div class="topline">
            <span class="id">ID</span>
            <span class="title">Title</span>
            <span class="vote">Vote</span>
        </div>
        <div>
            
            <div class="votes" v-for="(proposal, index) in visibleItems" :key="index">
                <div class="id-vote">#{{ proposal.proposal_id }}</div> 
                <div class="title-vote">{{ proposal.content.title }}</div>
            </div>
                    
            <div class="pages">
                <img src="src/assets/images/angel_left.svg" class="previous" @click="previousPage" v-if="currentPage != 1"/>
                <span class="nb-pages">{{ currentPage }}/{{ totalPages }}</span>
                <img src="src/assets/images/angel_right.svg" class="next" @click="nextPage" v-if="currentPage != totalPages"/>
            </div>
        </div>
    </div>
  

</template>

<script lang="ts">

import axios from 'axios';
import type { AxiosResponse } from 'axios';

interface VotesArray {
    proposals: {
        id: number,
        title: string,
        description: string
    }
}

export default ({
  data() {
    return {
      items: ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12'] as string[], // replace with your actual data
      itemsPerPage: 5 as number,
      currentPage: 1 as number,
      APIurl: 'https://evmos-api.polkachu.com/cosmos/gov/v1beta1/proposals' as string,
      allProposals: [] as string[],
      url: '' as string || null,
    };
  },

  computed: {
    totalPages(): number {
      return Math.ceil(this.allProposals.length / this.itemsPerPage);
    },

    startIndex(): number {
      return (this.currentPage - 1) * this.itemsPerPage;
    },

    endIndex(): number {
      return this.startIndex + this.itemsPerPage;
    },

    visibleItems(): string[] {
      return this.allProposals.slice(this.startIndex, this.endIndex);
    },
  },

  methods: {
    nextPage(): void {
      this.currentPage++;
    },

    previousPage(): void {
        this.currentPage--;
    },

    async fetchVoteData() {
      try {

        this.url = this.APIurl;
        //let allProposals= [];
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
    this.fetchVoteData();
  },
});
</script>

<style lang="scss">

    $width: 649px;

    .container{
        width: $width;
        border: 1px solid rgba(70, 70, 70, 1);
        margin: auto;
        margin-top: 20px;
        text-align: left;
        padding-left: 8px;
    }

    .topline{
        height: 30px;
    }

    .head{
        font-weight: 600;
        font-style: 16px;
        height: 32px;
    }

    .id{
        margin-right: 50px;
    }

    .title{
        margin-right: 475px;
        
    }

    .votes{
        font-weight: 400;
        height: 24px;
        margin-bottom: 5px;
    }

    .id-vote{
        display: inline-block;
        width: 65px;
        overflow: hidden;
    }

    .title-vote{
        display: inline-block;
        width: 500px;
        overflow: hidden;
        white-space: nowrap;

        &:hover {
            overflow:auto;
            margin-top: -2px;
        }
    }

    .title-vote::-webkit-scrollbar {
        height: 2px;
    }

    .title-vote::-webkit-scrollbar-thumb {
        background-color: rgb(87, 86, 86); /* Adjust as needed */
    }

    .previous, .next{
        width: 14px;
    }

    .pages{
        //text-align: end;
        //margin-right: 10px;
        margin-left: 550px;
    }

    .nb-pages{
        margin-left: 0px;
    }

    .hidden {
        visibility: hidden;
    }

</style>