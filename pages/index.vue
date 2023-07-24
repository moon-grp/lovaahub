<template>
  <div id="bg">
    <v-row class="mt-10 hidden-sm-and-down">
      <v-card class="mx-auto py-8" width="544" outlined>
        <v-col>
          <v-row justify="center" align="center" class="">
            <v-col cols="12" sm="8" md="6">
              <p class="text-capitalize text-center titl-fnt">
                Login to Admin Dashboard
              </p>
            </v-col>
          </v-row>
          <!--       <v-row class="mt-4">
            <v-col>
              <div>
                <v-text-field
                  label="Enter your Email"
                  style="corner"
                  outlined
                  color="#45D0FB"
                  class="{width:40px}"
                  v-model="email"
                  :error-messages="emailErrors"
                  @input="$v.email.$touch()"
                  @blur="$v.email.$touch()"
                ></v-text-field>
              </div>
            </v-col>
          </v-row> -->

          <v-row>
            <v-col>
              <v-text-field
                label="Enter your Password"
                outlined
                color="#45D0FB"
                v-model="password"
                :error-messages="passwordErros"
                @input="$v.password.$touch()"
                @blur="$v.password.$touch()"
                @click:append="show = !show"
                :type="show ? 'text' : 'password'"
                :append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
              ></v-text-field>
            </v-col>
          </v-row>

          <v-row>
            <v-col>
              <div>
                <v-btn
                  color="#252362"
                  x-large
                  :loading="loading"
                  dark
                  @click="login"
                  block
                  class="text-capitalize"
                >
                  log in
                </v-btn>
              </div>
            </v-col>
          </v-row>
        </v-col>
      </v-card>
    </v-row>



    <!-- mobile version -->
    <v-row class="hidden-md-and-up mt-2">
      <v-card class="mx-4 " width="544" outlined>
        <v-col>
          <v-row justify="center" align="center" class="">
            <v-col cols="12" sm="8" md="6">
              <p class="text-capitalize text-center titl-fnt">
                Login to Admin Dashboard
              </p>
            </v-col>
          </v-row>
          <!--       <v-row class="mt-4">
            <v-col>
              <div>
                <v-text-field
                  label="Enter your Email"
                  style="corner"
                  outlined
                  color="#45D0FB"
                  class="{width:40px}"
                  v-model="email"
                  :error-messages="emailErrors"
                  @input="$v.email.$touch()"
                  @blur="$v.email.$touch()"
                ></v-text-field>
              </div>
            </v-col>
          </v-row> -->

          <v-row>
            <v-col>
              <v-text-field
                label="Enter your Password"
                outlined
                color="#45D0FB"
                v-model="password"
                :error-messages="passwordErros"
                @input="$v.password.$touch()"
                @blur="$v.password.$touch()"
                @click:append="show = !show"
                :type="show ? 'text' : 'password'"
                :append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
              ></v-text-field>
            </v-col>
          </v-row>

          <v-row>
            <v-col>
              <div>
                <v-btn
                  color="#252362"
                  x-large
                  :loading="loading"
                  dark
                  @click="login"
                  block
                  class="text-capitalize"
                >
                  log in
                </v-btn>
              </div>
            </v-col>
          </v-row>
        </v-col>
      </v-card>
    </v-row>
    <v-snackbar v-model="snackbar" :timeout="timeout" color="success" top>
      {{ msg }}
    </v-snackbar>
    <v-snackbar v-model="snackbarErr" :timeout="timeout" color="error" top>
      {{ msg }}
    </v-snackbar>
  </div>
</template>

<script>
import axios from 'axios'
import { validationMixin } from 'vuelidate'
import {
  required,
  minLength,
  maxLength,
  email,
  sameAs,
} from 'vuelidate/lib/validators'
export default {
  layout: 'defaultHome',
  mixins: [validationMixin],
  validations: {
    // email: { required },
    password: { required },
  },
  data() {
    return {
      timeout: 7000,
      email: '',
      password: '',
      msg: '',
      snackbarErr: false,
      snackbar: false,
      loading: false,
      show: false,
    }
  },
  methods: {
    async login() {
      this.$v.$touch()

      if (!this.$v.$invalid) {
        this.loading = true

        if (this.password == this.$config.loginPassword) {
          this.loading = false
          this.$router.push({ name: 'dashboard' })
        } else {
          this.loading = false
          this.msg = 'wrong password, try again.'
          this.snackbarErr = true
        }

        // this.getWorkId(this.staffId)
      }
    },
  },
  computed: {
    passwordErros() {
      const errors = []
      if (!this.$v.password.$dirty) return errors
      !this.$v.password.required && errors.push('Your password is required.')
      return errors
    },
    emailErrors() {
      const errors = []
      if (!this.$v.email.$dirty) return errors
      !this.$v.email.required && errors.push('Your email is required')
      return errors
    },
  },
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Lato:wght@300&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Urbanist:wght@500&display=swap');

.xsx {
  margin-top: 20%;
}
.fnt {
  font-family: 'Lato', sans-serif;
}
#sht {
  padding-top: 80px;
}
.rad.v-text-field {
  border-radius: 8px;
  width: 420px;
}

.rad.v-text-field--outlined >>> fieldset {
  border-color: #5465ff;
}

.text-c {
  color: #f8ec14;
}

.rad.v-btn {
  border-radius: 6px;
  width: 160px;
}
#bg {
  background: #ffffff;
  height: 100vh;
}

#cent {
  margin-top: 6%;
}

#cent-mb {
  margin-top: 4%;
}
.titl-fnt {
  font-family: 'Urbanist', sans-serif;
  color: #252362;
  font-size: 22px;
}

.sub-fnt {
  font-family: 'Urbanist', sans-serif;
  font-size: 22px;
  font-weight: lighter;
}

.titl-fnt-mb {
  font-family: 'Urbanist', sans-serif;
  font-size: 20px;
  font-weight: bold;
}

.sub-fnt-mb {
  font-family: 'Urbanist', sans-serif;
  font-size: 12px;
  font-weight: lighter;
}
</style>
