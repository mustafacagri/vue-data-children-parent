<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <span class="font-weight-bold">Data transferred between children and parents (Emit, Props)</span>
      </div>

      <v-spacer></v-spacer>

      <v-btn href="https://github.com/mustafacagri/vue-data-children-parent" target="_blank" text>
        <span class="mr-2">Go to Github Repo</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <Home />
    </v-main>
  </v-app>
</template>

<script>
import Home from './components/Home'

export default {
  name: 'App',

  components: {
    Home
  },

  data: () => ({
    //
  })
}
</script>
