<template>
  <div>
    <v-row class="mt-2 ml-8 titl-fnt">Data Bundle </v-row>
    <v-row class="mt-10 mx-auto" justify="center">
      <v-select
        :items="items"
        label="Network"
        outlined
        v-model="network"
        item-text="text"
        item-value="value"
        @change="getNetwork"
      ></v-select>
    </v-row>
    <v-row class="mt-2" v-if="mtn_">
      <v-col>
        <v-select
          :items="mtn"
          label="Data plan mtn"
          outlined
          v-model="t"
          item-text="text"
          item-value="value"
        ></v-select>
      </v-col>
    </v-row>
    <v-row class="mt-2" v-if="glo_">
      <v-col>
        <v-select
          :items="glo"
          label="Data plan glo"
          outlined
          v-model="t"
          item-text="text"
          item-value="value"
        ></v-select>
      </v-col>
    </v-row>
    <v-row class="mt-2" v-if="airtel_">
      <v-col>
        <v-select
          :items="airtel"
          label="Data plan airtel"
          outlined
          v-model="t"
          item-text="text"
          item-value="value"
        ></v-select>
      </v-col>
    </v-row>
    <v-row class="mt-2" v-if="nine_">
      <v-col>
        <v-select
          :items="nineMobile"
          label="Data plan 9mobile"
          outlined
          v-model="t"
          item-text="text"
          item-value="value"
        ></v-select>
      </v-col>
    </v-row>
    <v-row class="mt-2">
      <v-col>
        <div>
          <v-text-field
            label="Phone Number"
            style="corner"
            outlined
            color="#45D0FB"
            class="{width:40px}"
            v-model="phone"
            type="number"
            :error-messages="phoneErrors"
            @input="$v.phone.$touch()"
            @blur="$v.phone.$touch()"
          ></v-text-field>
        </div>
      </v-col>
    </v-row>

    <!--  <v-row>
      <v-col>
        <v-text-field
          label="Enter Amount"
          outlined
          color="#45D0FB"
          v-model="amount"
          type="number"
          :error-messages="amountErros"
          @input="$v.amount.$touch()"
          @blur="$v.amount.$touch()"
          hint="Amount cant be less than 100"
        ></v-text-field>
      </v-col>
    </v-row> -->

    <v-row>
      <v-col>
        <div>
          <v-btn
            color="#252362"
            x-large
            :loading="loading"
            dark
            @click="getDialog"
            block
            class="text-capitalize"
          >
            buy data
          </v-btn>
        </div>
      </v-col>
    </v-row>

    <v-dialog
      transition="dialog-bottom-transition"
      max-width="600"
      v-model="dialog"
    >
      <template v-slot:default="dialog">
        <v-card>
          <v-card-text>
            <div class="text-h4 pa-4">
              confirm sending {{ t }} data to {{ phone }}
            </div>
          </v-card-text>
          <v-card-actions class="justify-end">
            <v-btn @click="buyAirtime" outlined color="green">Yes</v-btn>
            <v-btn @click="dialog.value = false" outlined color="red">No</v-btn>
          </v-card-actions>
        </v-card>
      </template>
    </v-dialog>

    <v-snackbar v-model="snackbar" :timeout="timeout" color="success" top>
      {{ msg }}
    </v-snackbar>
    <v-snackbar v-model="snackbarErr"  color="error" top >
      {{ msg }}
      <template v-slot:action="{ attrs }">
        <v-btn
          color="black"
          text
          v-bind="attrs"
          @click="snackbarErr = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </div>
</template>

<script>
import { validationMixin } from 'vuelidate'
import { required } from 'vuelidate/lib/validators'
export default {
  layout: 'dashboard',
  mixins: [validationMixin],
  validations: {
   // amount: { required },
    phone: { required },
  },
  data() {
    return {
      mtn_: true,
      glo_: false,
      airtel_: false,
      nine_: false,
      t: '',
      dialog: false,
      tableBody: [],
      items: [
        { text: 'mtn', value: 'mtn' },
        { text: 'glo', value: 'glo' },
        { text: 'airtel', value: 'airtel' },
        { text: '9mobile', value: '9mobile' },
      ],
      amount: '',
      phone: '',
      loading: false,
      network: '',
      timeout: 10000,
      snackbarErr: false,
      snackbar: false,
      msg: '',
      mtn: [
        { text: '500MB - 30 Days (₦ 149)', value: '500' },
        { text: '1GB - 30 Days (₦ 249)', value: 'M1024' },
        { text: '2GB - 30 Days (₦ 499)', value: 'M2024' },
        { text: '3GB - 30 Days (₦ 749)', value: '3000' },
        { text: '5GB - 30 Days (₦ 1,249)', value: '5000' },
        { text: '6GB - 7 Days (₦ 1,489)', value: 'mtn-20hrs-1500' },
        { text: '10GB - 30 Days (₦ 2,499)', value: '10000' },
        { text: '40GB - 30 Days (₦ 9,859)', value: 'mtn-40gb-10000' },
        { text: '75GB - 30 Days (₦ 14,899)', value: 'mtn-75gb-15000' },
      ],
      glo: [
        { text: '1GB - 5 nights', value: 'glo100x' },
        { text: 'Glo Data 1.25GB – 1 Day (Sunday)', value: 'glo200x' },
        { text: 'Glo Data 1.35GB – 14 Days', value: 'G500' },
        { text: 'Glo Data 2.9GB – 30 Days', value: 'G1000' },
        { text: 'Glo Data 5.8GB – 30 Days', value: 'G2000' },
        { text: 'Glo Data 7.7GB – 30 Days', value: 'G2500' },
        { text: 'Glo Data 10GB – 30 Days', value: 'G3000' },
        { text: 'Glo Data 13.25GB – 30 Days', value: 'G4000' },
        { text: 'Glo Data 18.25GB – 30 Days', value: 'G5000' },
        { text: 'Glo Data 29.5GB – 30 Days', value: 'G8000' },
        { text: 'Glo Data 50GB – 30 Days', value: 'glo10000' },
      ],
      airtel: [
        { text: 'Airtel Data 500MB (Gift) – 30 Days', value: 'AIRTEL500MB' },
        { text: 'Airtel Data 1GB (Gift) – 30 Days', value: 'AIRTEL1GB' },
        { text: 'Airtel Data 2GB (Gift)– 30 Days', value: 'AIRTEL2GB' },
        { text: 'Airtel Data 5GB (Gift)– 30 Days', value: 'AIRTEL5GB' },
        { text: 'Airtel Data 10GB (Gift)– 30 Days', value: 'AIRTEL10GB' },
        { text: 'Airtel Data 15GB (Gift)– 30 Days', value: 'AIRTEL15GB' },
        { text: 'Airtel Data 20GB (Gift)– 30 Days', value: 'AIRTEL20GB' },
        { text: 'Airtel Data 1.5GB – 30 Days', value: 'airt-1100' },
        { text: 'Airtel Data 2GB – 30 Days', value: 'airt-1300' },
        { text: 'Airtel Data 3GB – 30 Days', value: 'airt-1650' },
        { text: 'Airtel Data 4.5GB – 30 Days', value: 'airt-2200' },
        { text: 'Airtel Data 10GB – 30 Days', value: 'airt-3300' },
        { text: 'Airtel Data 20GB – 30 Days', value: 'airt-5500' },
        { text: 'Airtel Data 40GB – 30 Days', value: 'airt-11000' },
        { text: 'Airtel Data 1GB – 1 Day', value: 'airt-330x' },
        { text: 'Airtel Data 750MB – 14 Days', value: 'airt-550' },
        { text: 'Airtel Data 6GB – 7 Days', value: 'airt-1650-2' },
      ],
      nineMobile: [
        { text: '9mobile Data 1GB – 30 Days', value: '9MOB1000' },
        { text: '9mobile Data 2.5GB – 30 Days', value: '9MOB34500' },
        { text: '9mobile Data 11.5GB – 30 Days', value: '9MOB8000' },
        { text: '9mobile Data 15GB – 30 Days', value: '9MOB5000' },
      ],
    }
  },
  methods: {
    tryy() {
      alert(this.t)
    },
    getNetwork() {
      if (this.network == 'mtn') {
        this.mtn_ = true
        this.glo_ = false
        this.airtel_ = false
        this.nine_ = false
      }

      if (this.network == 'glo') {
        this.glo_ = true
        this.mtn_ = false
        this.airtel_ = false
        this.nine_ = false
      }

      if (this.network == 'airtel') {
        this.airtel_ = true
        this.glo_ = false
        this.mtn_ = false
        this.nine_ = false
      }

      if (this.network == '9mobile') {
        this.nine_ = true
        this.glo_ = false
        this.mtn_ = false
        this.airtel_ = false
      }
    },
    getDialog() {
      this.$v.$touch()
      if (!this.$v.$invalid) {
        this.loading = true
        this.dialog = true
        this.loading = false
      }
    },
    async buyAirtime() {
      try {
        const res = await this.$axios.$get(`${this.$config.baseUrl}data?`, {
          params: {
            username: this.$config.username,
            password: this.$config.password,
            network_id: this.network,
            // amount: this.amount,
            phone: this.phone,
            variation_id: this.t,
          },
        })
        console.log(res)
        this.msg = res.message
        this.snackbar = true
        this.dialog = false
      } catch (error) {
        // console.log(error.response.data.message)
        this.dialog = false
        this.msg = error.response.data.message
        this.snackbarErr = true
      }
    },
  },
  created() {
    //this.getDeliveries()
  },
  computed: {
    amountErros() {
      const errors = []
      if (!this.$v.amount.$dirty) return errors
      !this.$v.amount.required && errors.push('An amount is required.')
      return errors
    },
    phoneErrors() {
      const errors = []
      if (!this.$v.phone.$dirty) return errors
      !this.$v.phone.required && errors.push('Phonenumber is required')
      return errors
    },
  },
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Lato:wght@300&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Urbanist:wght@500&display=swap');
.titl-fnt {
  font-family: 'Urbanist', sans-serif;
  font-size: 30px;
  font-weight: bold;
}
</style>
