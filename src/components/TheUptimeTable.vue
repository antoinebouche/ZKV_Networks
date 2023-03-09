<template>
    <p>{{uptime}}</p>
    <p>{{latency}}</p>
</template>



<script lang="ts">


    export default {

        name: 'TheUptimeTable',
        props: {
            uptime: Number,
            latency: Number
        },
        methods: {

        }

    };
</script>

<style>

</style>