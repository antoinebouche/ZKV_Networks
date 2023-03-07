<template>
    <table class="grey-table">
        <thead>
            <tr class="first-row">
                <th>Network</th>
                <th>Chain ID</th>
                <th>Current Node Version</th>
            </tr>
        </thead>
        <tbody>
            <tr class="second-row">
                <td>Mainnet</td>
                <td>{{ nodeInfo.network }}</td>
                <td>{{networkVersion.version}}</td>
            </tr>
        </tbody>
    </table>
</template>

<script lang="ts">

    import axios from "axios";
    import type { AxiosResponse } from 'axios';


    interface node_info {
        network: string;
    };

    interface network_version {
        version: string;
    };
  
export default {

    name: 'TheNetworkInfo',
    data() {
        return{
            nodeInfo: {} as node_info,
            networkVersion: {} as network_version
        };
    },

    methods:{
        async fetchNetworkInfo(): Promise<void> {
            try{
                const response: AxiosResponse<any> = await axios.get('https://evmos-api.polkachu.com/cosmos/base/tendermint/v1beta1/node_info');
                this.nodeInfo.network = response.data.default_node_info.network;
                this.networkVersion.version = response.data.application_version.version;
            }
            catch (error) {
                console.error(error);
            }
        }
    },

    mounted() {
        this.fetchNetworkInfo();
  }

}
</script>

<style lang="scss">

    $width: 661px;
    $grey: #B8B8B8;
    $black: #111;

    .grey-table{
        width:$width;
        margin: auto;
        box-shadow: 0 0 0 3px $grey;
        border-collapse: collapse;
        border-style: hidden;
        border-radius: 8px;
        margin-top: 55px;
        table-layout: fixed;
    }

    th{
        background-color: $grey;
        color: $black;
        font-weight: 600;
    }

    th, tr, td{
        border: solid 3px $grey;
    }

    tr {
        font-weight: 400;
    }

</style>