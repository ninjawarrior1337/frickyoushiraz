<template>
    <v-layout fill-height align-center>
        <v-layout align-center justify-space-around column>
            <h1>What, exactly, is this?</h1>
            <h2>Have you ever wanted to know exactly where a train was on the LA Metro System</h2>
            <h2>Well now you can, thanks to the Metro API and Vue.js and Nuxt</h2>
            <v-layout row align-center justify-center>
                <v-card class="ma-4 pa-1">
                    <v-img width=300 src="https://image.flaticon.com/icons/svg/25/25231.svg"/>
                    <v-card-actions>
                        <v-btn href="https://github.com/ninjawarrior1337/frickyoushrizzle" target="_blank" flat color="black">
                            Visit GitHub Page
                        </v-btn>
                    </v-card-actions>
                </v-card>
                <v-card class="ma-4 pa-1">
                    <v-img width=300 src="https://raw.githubusercontent.com/vuejs/art/master/logo.png"/>
                    <v-card-actions>
                        <v-btn href="https://vuejs.org/" target="_blank" flat color="#42b883">
                            Visit Vue Website
                        </v-btn>
                    </v-card-actions>
                </v-card>
                <v-card class="ma-4 pa-1">
                    <v-img width=300 height=300 src="https://seeklogo.com/images/N/nuxt-logo-5EF50E1ABD-seeklogo.com.png"/>
                    <v-card-actions>
                        <v-btn href="https://nuxtjs.org/" target="_blank" flat color="#42b883">
                            Visit Nuxt Website
                        </v-btn>
                    </v-card-actions>
                </v-card>
            </v-layout>
            <v-btn nuxt to="/" large dark>Go Back</v-btn>
        </v-layout>
    </v-layout>
</template>

<script>
export default {
    components: {

    },
    transition: {
        name: 'about',
        enterActiveClass: "animated zoomIn",
        leaveActiveClass: "animated zoomOut"
    }
}
</script>

<style>

</style>
