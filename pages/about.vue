<template>
    <v-layout fill-height align-center justify-space-around column>
        <h1>What is this?</h1>
        <h2>Have you ever wanted to know exactly where a train was on the LA Metro System</h2>
    </v-layout>
</template>

<script>
export default {
    transition: {
        name: 'about',
        enterActiveClass: "animated zoomIn",
        leaveActiveClass: "animated zoomOut"
    }
}
</script>

<style>

</style>
