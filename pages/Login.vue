<template>
    <v-app>
      <div>
        <v-content>
          <v-container>
              <h2>ログイン</h2>
            <v-form ref="form" v-model="valid" lazy-validation class="='mt-16">
              <v-text-field
                v-model="userId"
                :counter="20"
                :rules="nameRules"
                label="user id"
                required
              ></v-text-field>
              <v-text-field
                v-model="password"
                :rules="emailRules"
                label="password"
                required
              ></v-text-field>
              <v-btn
                color="blue lighten-2"
                class="mr-4"
                @click="validate"
              >
                LogIn
              </v-btn>
              <v-btn color="error" class="mr-4" @click="reset">
                Reset Form
              </v-btn>
            </v-form>
          </v-container>
        </v-content>
      </div>
    </v-app>
  </template>
  <script lang='ts'>
  export default {
    layout: "auth",
    data: () => ({
      valid: true,
      userId: "" as String,
      nameRules: [
        (v) => !!v || "Name is required",
        (v) => (v && v.length <= 10) || "Name must be less than 10 characters",
      ],
      password: "",
      emailRules: [
        (v) => !!v || "E-mail is required",
        (v) => /.+@.+\..+/.test(v) || "E-mail must be valid",
      ],
      select: null,
      items: ["Item 1", "Item 2", "Item 3", "Item 4"],
      checkbox: false,
    }),
  
    methods: {
      validate() {
        this.$refs.form.validate();
      },
      reset() {
        this.$refs.form.reset();
      },
      resetValidation() {
        this.$refs.form.resetValidation();
      },
    },
  };
  </script>