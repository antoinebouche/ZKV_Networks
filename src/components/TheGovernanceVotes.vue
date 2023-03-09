<template>
    <div class="container">
        <div class="head">Governance votes</div>
        <div class="topline">
            <span class="id">ID</span>
            <span class="title">Title</span>
            <span class="vote">Vote</span>
        </div>
        <div>
            <div class="votes-container">
                <div class="votes" v-for="(proposal, index) in visibleItems" :key="index">
                    <a :href="`https://www.mintscan.io/evmos/proposals/${proposal.proposal_id}`" target="_blank">
                        <div class="id-vote">#{{ proposal.proposal_id }}</div> 
                        <div class="title-vote">{{ proposal.content.title }}</div>
                    </a>
                </div>
            </div>      
            <div class="pages">
                <img src="src/assets/images/angel_left.svg" class="previous" @click="previousPage" v-show="currentPage>1"/>
                <div class="placeholder" v-show="currentPage == 1"></div>
                <span class="nb-pages">{{ currentPage }}/{{ totalPages }}</span>
                <img src="src/assets/images/angel_right.svg" class="next" @click="nextPage" v-show="currentPage < totalPages"/>
            </div>
        </div>
    </div>
  

</template>

<script lang="ts">

export default {
  data() {
    return {
      itemsPerPage: 5 as number,
      currentPage: 1 as number,
      APIurl: 'https://evmos-api.polkachu.com/cosmos/gov/v1beta1/proposals' as string,
      url: '' as string || null,
    };
  },

  props: {
    allProposals: Array<string>
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
  },

};
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
        margin-bottom: 36px;
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

    .votes-container{
        height: 140px;
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
        width: 490px;
        overflow: hidden;
        white-space: nowrap;
        font-size: 14px;
        color: #79A1FF;

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

    .previous, .next, .placeholder{
       width: 14px;
       height: 14px;
       display: inline-block;
    }

    .pages{
        margin-left: 550px;
        display: flex;
        align-items: center;
        margin-bottom: 6px;
    }

    .nb-pages{
        width: 50px;
        display: inline-block;
        text-align: center;
    }


</style>