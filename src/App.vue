<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2 white--text"
          contain
          src="/img/calculator.svg"
          transition="scale-transition"
          width="40"
        />
          <span class="pr-2 text-h6 overline">
            محاسبه سود
          </span>
      </div>

      <v-spacer></v-spacer>

    </v-app-bar>

    <v-main>
      <keep-alive>
        <router-view/>
      </keep-alive>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    //
  })
}
</script>
