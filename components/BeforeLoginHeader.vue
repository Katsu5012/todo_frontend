<template>
  <div>
    <v-app-bar color="blue lighten-2" dark>
      <v-toolbar-title>ToDo</v-toolbar-title>
      <v-spacer></v-spacer>
      <NuxtLink to="/Home">
        <v-btn class="mr-3" outlined> Home </v-btn>
      </NuxtLink>
      <NuxtLink to="/Login">
        <v-btn class="mr-3" outlined> Login </v-btn>
      </NuxtLink>
      <NuxtLink to="/register">
        <v-btn class="mr-3" outlined> Register </v-btn>
      </NuxtLink>
    </v-app-bar>
  </div>
</template>
<style scoped>
a {
  text-decoration: none;
}
</style>
