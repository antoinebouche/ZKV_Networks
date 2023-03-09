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
                <td>{{ nodeInfo }}</td> 
                <td>{{networkVersion}}</td>
            </tr>
        </tbody>
    </table>
</template>

<script lang="ts">
  
export default {

    name: 'TheNetworkInfo',

    props: {
        nodeInfo: {type: String },
        networkVersion: {type : String},

    },
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
        margin-bottom: 36px;
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