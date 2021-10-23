<template>
  <v-app>
    <BeforeLoginHeader />
    <v-content
      style="background: #f5f5f5; height: auto"
      class="pa-0 fill-height"
    >
      <v-container>
        <v-row justify="center" align-content="center">
          <v-col cols="6">
            <nuxt />
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>