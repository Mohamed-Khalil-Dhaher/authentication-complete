<template>
  <v-card class="pa-6">
    <v-card-title>Sign Up</v-card-title>
    <v-form ref="form" v-model="valid" lazy-validation>
      <v-text-field
        v-model="firstName"
        :counter="10"
        :rules="firstnameRules"
        label="First Name"
        required
      ></v-text-field>

      <v-text-field
        v-model="lastName"
        :counter="10"
        :rules="lastNameRules"
        label="Last Name"
        required
      ></v-text-field>

      <v-text-field
        v-model="phoneNumber"
        :counter="10"
        :rules="phoneNumberRules"
        label="Phone Number"
        required
      ></v-text-field>

      <v-text-field
        v-model="email"
        :rules="emailRules"
        label="E-mail"
        required
      ></v-text-field>
      <v-text-field
        v-model="bio"
        :counter="90"
        :rules="bioRules"
        label="Bio"
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
        Register
      </v-btn>

      <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
    </v-form>
  </v-card>
</template>
<script>
import { mapActions } from 'vuex'
export default {
  data: () => ({
    valid: true,
    firstName: '',
    lastName: '',
    phoneNumber: '',
    firstnameRules: [
      (v) => !!v || 'firstName is required',
      (v) =>
        (v && v.length <= 10) || 'firstName must be less than 10 characters',
    ],
    lastNameRules: [
      (v) => !!v || 'Last Name is required',
      (v) =>
        (v && v.length <= 10) || 'Last Name must be less than 10 characters',
    ],
    phoneNumberRules: [
      (v) => !!v || 'Phone Number is required',
      (v) =>
        (v && v.length <= 10) || 'Phone Number must be less than 9 characters',
    ],
    email: '',
    emailRules: [
      (v) => !!v || 'E-mail is required',
      (v) => /.+@.+\..+/.test(v) || 'E-mail must be valid',
    ],
    bio: '',
    bioRules: [
      (v) => (v && v.length > 0) || 'bio must be gretter than 0 characters',
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
          firstName: this.firstName,
          lastName: this.lastName,
          phoneNumber: this.phoneNumber,
          email: this.email,
          bio: this.bio,
          password: this.password,
        }
        let rtn = await this.$axios.$post('/api/register', userAuth)
        console.log({ userAuth, rtn })
        this.$router.push('/login')
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
