<template>
  <div>
    <v-row class="mt-2 ml-8 titl-fnt">Electricity recharge </v-row>
    <v-row class="mt-10 mx-auto" justify="center">
      <v-select
        :items="items"
        item-text="text"
        item-value="value"
        label="Electricity Company"
        outlined
        v-model="electricCompany"
      ></v-select>
    </v-row>
    <v-row class="mt-2">
      <v-col>
        <v-select
          :items="meter"
          label="Meter type"
          outlined
          v-model="meterType"
        ></v-select>
      </v-col>
    </v-row>
    <v-row class="mt-4">
      <v-col>
        <div>
          <v-text-field
            label="Meter Number"
            style="corner"
            outlined
            color="#45D0FB"
            class="{width:40px}"
            v-model="meterNumber"
           
          ></v-text-field>
        </div>
      </v-col>
    </v-row>

    <v-row>
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
    </v-row>

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
            buy electricity
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
              confirm {{ amount }} worth of units from {{ electricCompany }}
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
    amount: { required },
   // phone: { required },
  },
  data() {
    return {
      electricCompany: '',
      meterNumber: '',
      meterType: '',
      dialog: false,
      meter: ['prepaid', 'postpaid'],
      items: [
        {
          text: 'Abuja Electricity Distribution Company (AEDC)',
          value: 'abuja-electric',
        },
        {
          text: 'Benin Electricity Distribution Company (BEDC)',
          value: 'benin-electric',
        },
        {
          text: 'Eko Electricity Distribution Company (EKEDC)',
          value: 'eko-electric',
        },
        {
          text: 'Enugu Electricity Distribution Company (EEDC)',
          value: 'enugu-electric',
        },
        {
          text: 'Ibadan Electricity Distribution Company (IBEDC)',
          value: 'ibadan-electric ',
        },
        {
          text: 'Ikeja Electricity Distribution Company (IKEDC)',
          value: 'ikeja-electric',
        },
        {
          text: 'Jos Electricity Distribution PLC (JEDplc)',
          value: 'jos-electric',
        },
        {
          text: 'Kaduna Electricity Distribution Company (KAEDCO)',
          value: 'kaduna-electric',
        },
        {
          text: 'Kano Electricity Distribution Company (KEDCO)',
          value: 'kano-electric',
        },
        {
          text: 'Port Harcourt Electricity Distribution Company (PHED)',
          value: 'portharcourt-electric',
        },
      ],
      amount: '',
      phone: '',
      loading: false,
      network: '',
      timeout: 10000,
      snackbarErr: false,
      snackbar: false,
      msg: '',
    }
  },
  methods: {
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
        const res = await this.$axios.$get(
          `${this.$config.baseUrl}electricity?`,
          {
            params: {
              username: this.$config.username,
              password: this.$config.password,
              phone: '08131668189',
              meter_number: this.meterNumber,
              service_id: this.electricCompany,
              variation_id: this.meterType,
              amount: this.amount,
            },
          }
        )
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
