<template>
  <v-container fluid class="my-3">
    <v-row align="center" justify="center">
      <v-col cols="12">
        <v-card style="width: 100%" flat>
          <div class="text-center pt-6"><h1 class="font-weight-light display-2 primary--text">Register!</h1></div>
          <v-row class="pa-4">
            <v-col cols="12" md="6">
              <v-img src="@/assets/contactus.jpg" class="d-block ml-auto mr-auto" max-width="350px" />
            </v-col>
            <v-col cols="12" md="6">
              <v-form ref="forma" v-model="form1" lazy-validation>
                <v-row>
                  <v-col
                    cols="12"
                    md="6"
                  >
                    <v-text-field
                      v-model="first_name"
                      label="First Name"
                      outlined
                      dense
                      hide-details
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="6"
                  >
                    <v-text-field
                      v-model="last_name"
                      label="Last Name"
                      outlined
                      dense
                      hide-details
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="6"
                  >
                    <v-text-field
                      v-model="phone"
                      label="Phone"
                      outlined
                      dense
                      hide-details
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="6"
                  >
                    <v-text-field
                      v-model="email"
                      label="Email"
                      outlined
                      dense
                      hide-details
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="6"
                  >
                    <v-text-field
                      v-model="age"
                      type="number"
                      label="Age"
                      outlined
                      dense
                      hide-details
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="6"
                  >
                    <v-file-input
                      v-model="photo"
                      :rules="photoRules"
                      accept="image/png, image/jpeg"
                      label="Pick a Picture"
                      outlined
                      dense
                      prepend-icon=""
                      prepend-inner-icon="mdi-camera"
                      hide-details
                    >
                    </v-file-input>
                  </v-col>
                  <v-col
                    cols="12"
                    md="6"
                  >
                    <v-text-field
                      v-model="password"
                      :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                      :rules="passwordRules"
                      :type="show1 ? 'text' : 'password'"
                      hint="At least 6 characters"
                      @click:append="show1 = !show1"
                      outlined
                      dense
                      hide-details
                    >
                    </v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="6"
                  >
                    <v-text-field
                      v-model="confirmPassword"
                      :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                      :rules="passwordConfirmation"
                      :type="show1 ? 'text' : 'password'"
                      hint="At least 6 characters"
                      @click:append="show1 = !show1"
                      outlined
                      dense
                      hide-details
                    >
                    </v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="12"
                  >
                    <v-text-field
                      v-model="address"
                      label="Address"
                      outlined
                      dense
                      hide-details
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                  >
                    <v-btn
                      color="primary"
                      rounded
                      block
                      class="mt-3"
                      @click="onSubmit"
                      :disabled="!form1"
                    >
                      Submit
                    </v-btn>
                  </v-col>
                </v-row>
              </v-form>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
    <v-snackbar
      v-model="snackbar"
      :timeout="timeout"
      color="success"
      top
    >
      {{ message }}
    </v-snackbar>
  </v-container>
</template>

<script>
import axios from 'axios'
const url = 'http://localhost:5005/'
export default {
  data () {
    return {
      message: 'submission success',
      snackbar: false,
      timeout: 4000,
      show1: false,
      form1: false,
      submitSuccess: false,
      first_name: '',
      last_name: '',
      phone: '',
      email: '',
      age: '',
      photo: null,
      address: '',
      password: '',
      confirmPassword: '',
      photoRules: [v => !v || v.size < 5000000 || 'Image should be less than 5MB'],
      passwordRules: [v => !!v || 'Password is required',
        v => (v && v.length >= 6) || 'password should be 6 characters'],
      passwordConfirmation: [v => !!v || 'Password is required',
        v => (v === this.password) || 'password should match'],
      required: [value => !!value || 'Required.']
    }
  },
  computed: {
    photoValidation () {
      if (this.photo) {
        return true
      } else return false
    }
  },
  methods: {
    clearForm () {
      this.$refs.forma.reset()
    },
    async onSubmit () {
      if (this.$refs.forma.validate() && this.photoValidation) {
        const blob = this.photo
        const formData = new FormData()
        formData.append('first_name', this.first_name)
        formData.append('last_name', this.last_name)
        formData.append('photo', blob)
        formData.append('phone', this.phone)
        formData.append('email', this.email)
        formData.append('age', this.age)
        formData.append('password', this.password)
        formData.append('address', this.address)

        axios.post(url, formData).then((res) => {
          if (res.status === 201) {
            console.log(res)
            this.clearForm()
            this.snackbar = true
          }
        }).catch((err) => {
          console.log(err.response)
        })
      }
    }
  }
}
</script>

<style>

</style>
