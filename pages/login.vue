<template>
  <!-- <v-card class="pa-6">
    <v-card-title>Login Form</v-card-title>
    <v-form ref="form" v-model="valid" lazy-validation>
      <v-text-field
        v-model="email"
        :rules="emailRules"
        label="E-mail"
        required
      ></v-text-field>
      <v-text-field
        v-model="password"
        :counter="50"
        :rules="passwordRules"
        label="Password"
        type="password"
        required
      ></v-text-field>

      <v-btn :disabled="!valid" color="success" class="mr-4" @click="validate">
        Login
      </v-btn>

      <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
      <v-btn href="/api/auth/google">signin with google</v-btn>
    </v-form>
  </v-card> -->
  <v-container>
    <v-layout row class="text-xs-center">
      <v-flex
        xs3
        style="
          background-image: url('http://cdn.wallpapersafari.com/7/86/gqiGH7.jpg');
        "
      >
        <v-card height="500px"></v-card>
      </v-flex>
      <v-flex xs4 class="grey lighten-4">
        <v-container
          style="position: relative; top: 13%"
          class="text-xs-center"
        >
          <v-card flat>
            <v-card-title primary-title>
              <h4>Login</h4>
            </v-card-title>
            <v-form ref="form" v-model="valid" lazy-validation>
              <v-text-field
                v-model="email"
                :rules="emailRules"
                label="E-mail"
                required
              ></v-text-field>
              <v-text-field
                v-model="password"
                :counter="50"
                :rules="passwordRules"
                label="Password"
                type="password"
                required
              ></v-text-field>
              <v-card-actions>
                <v-btn
                  primary
                  large
                  block
                  :disabled="!valid"
                  color="success"
                  class="mr-4"
                  @click="validate"
                  >Login</v-btn
                >
                <v-btn href="/api/auth/google">signin with google</v-btn>
              </v-card-actions>
            </v-form>
          </v-card>
        </v-container>
      </v-flex>
    </v-layout>
  </v-container>
</template>
<script>
import { mapActions } from 'vuex'
export default {
  data: () => ({
    valid: true,
    email: '',
    emailRules: [
      (v) => !!v || 'E-mail is required',
      (v) => /.+@.+\..+/.test(v) || 'E-mail must be valid',
    ],
    password: '',
    passwordRules: [
      (v) => !!v || 'password is required',
      (v) => (v && v.length > 6) || 'password must be more than 6 characters',
    ],
  }),
  methods: {
    ...mapActions(['changeUser']),
    async validate() {
      if (this.$refs.form.validate()) {
        let userAuth = {
          email: this.email,
          password: this.password,
        }
        let rtn = await this.$axios.$post('/api/login', userAuth)
        console.log({ rtn })
        const user = await this.$axios.$get('/api/user')
        if (user.firstName) {
          this.changeUser(user)
        } else {
          this.changeUser({ firstName: false })
        }
        this.$router.push('/profile')
      }
    },
    reset() {
      this.$refs.form.reset()
    },
  },
}
</script>
