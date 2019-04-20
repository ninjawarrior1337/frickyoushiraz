<template>
  <div>
    <v-app>
      <v-toolbar app>
        <v-toolbar-title>
         Frick You Shrizzle
        </v-toolbar-title>

        <v-spacer></v-spacer>

        <v-btn v-if="$nuxt.$route.name == 'index'" @click="$nuxt.$emit('refresh-locations')" icon>
          <v-icon>refresh</v-icon>
        </v-btn>

        <v-btn v-if="$nuxt.$route.name !== 'about'" nuxt to="/about" icon>
          <v-icon>info</v-icon>
        </v-btn>
        <v-btn v-if="$nuxt.$route.name !== 'index'" nuxt to="/" icon>
          <v-icon>close</v-icon>
        </v-btn>

      </v-toolbar>
      <v-content>
        <nuxt />
      </v-content>
    </v-app>
  </div>
</template>

<style lang="stylus">

</style>
