<template>
  <div>
    <v-row class="mt-2 ml-8 titl-fnt">Subscribe cable Tv </v-row>
    <v-row class="mt-10 mx-auto" justify="center">
      <v-select
        :items="items"
        label="Cable Tv"
        outlined
        v-model="network"
        item-text="text"
        item-value="value"
        @change="getNetwork"
      ></v-select>
    </v-row>
    <v-row class="mt-2" v-if="dstv_">
      <v-col>
        <v-select
          :items="dstv"
          label="Dstv packages"
          outlined
          v-model="t"
          item-text="text"
          item-value="value"
        ></v-select>
      </v-col>
    </v-row>
    <v-row class="mt-2" v-if="gotv_">
      <v-col>
        <v-select
          :items="gotv"
          label="GoTV packages"
          outlined
          v-model="t"
          item-text="text"
          item-value="value"
        ></v-select>
      </v-col>
    </v-row>
    <v-row class="mt-2" v-if="startimes_">
      <v-col>
        <v-select
          :items="startimes"
          label="Startimes packages"
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
            label="SmartCard/IUC number"
            style="corner"
            outlined
            color="#45D0FB"
            class="{width:40px}"
            v-model="smartCard"
            type="number"
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
              confirm subscribing {{ t }} for {{ smartCard }}
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
    // phone: { required },
  },
  data() {
    return {
      smartCard: '',
      dstv_: true,
      gotv_: false,
      startimes_: false,
      t: '',
      dialog: false,
      tableBody: [],
      items: [
        { text: 'Dstv', value: 'dstv' },
        { text: 'Gotv', value: 'gotv' },
        { text: 'Startimes', value: 'startimes' },
      ],
      amount: '',
      phone: '',
      loading: false,
      network: '',
      timeout: 10000,
      snackbarErr: false,
      snackbar: false,
      msg: '',
      dstv: [
        { text: 'DStv Padi', value: 'dstv-padi' },
        { text: 'DStv Yanga', value: 'dstv-yanga' },
        { text: 'DStv Confam', value: 'dstv-confam' },
        { text: 'DStv Asia', value: 'dstv6' },
        { text: 'DStv Compact', value: 'dstv79' },
        { text: 'DStv Compact Plus', value: 'dstv7' },
        { text: 'DStv Premium', value: 'dstv3' },

        { text: 'DStv Premium Asia', value: 'dstv10' },
        { text: 'DStv Premium-French', value: 'dstv9' },
        { text: 'DStv Confam + ExtraView', value: 'confam-extra' },
        { text: 'DStv Yanga + ExtraView', value: 'yanga-extra' },
        { text: 'DStv Padi + ExtraView', value: 'padi-extra' },
        { text: 'DStv Compact + Asia', value: 'com-asia' },
        { text: 'DStv Compact + Extra View', value: 'dstv30' },
        { text: 'DStv Compact + French Touch', value: 'com-frenchtouch' },
        { text: 'DStv Premium – Extra View', value: 'dstv33' },
        { text: 'DStv Compact Plus – Asia', value: 'dstv40' },
        {
          text: 'DStv Compact + French Touch + ExtraView',
          value: 'com-frenchtouch-extra',
        },
        { text: 'DStv Compact + Asia + ExtraView', value: 'com-asia-extra' },

        { text: 'DStv Compact Plus + French Plus', value: 'dstv43' },
        {
          text: 'DStv Compact Plus + French Touch',
          value: 'complus-frenchtouch',
        },
        { text: 'DStv Compact Plus – Extra View', value: 'dstv45' },
        {
          text: 'DStv Compact Plus + FrenchPlus + Extra View',
          value: 'complus-french-extraview',
        },
        { text: 'DStv Compact + French Plus', value: 'dstv47' },
        { text: 'DStv Compact Plus + Asia + ExtraView', value: 'dstv48' },
        { text: 'DStv Premium + Asia + Extra View', value: 'dstv61' },
        { text: 'DStv Premium + French + Extra View', value: 'dstv62' },
        { text: 'DStv HDPVR Access Service', value: 'hdpvr-access-service' },
        { text: 'DStv French Plus Add-on', value: 'frenchplus-addon' },
        { text: 'DStv Asian Add-on', value: 'asia-addon' },
        { text: 'DStv French Touch Add-on', value: 'frenchtouch-addon' },
        { text: 'ExtraView Access', value: 'extraview-access' },
        { text: 'DStv French 11', value: 'french11' },
      ],
      gotv: [
        { text: 'GOtv Smallie', value: 'gotv-smallie' },
        { text: 'GOtv Jinja', value: 'gotv-jinja' },
        { text: 'GOtv Jolli', value: 'gotv-jolli' },
        { text: 'GOtv Max', value: 'gotv-max' },
        { text: 'GOtv Supa', value: 'gotv-supa' },
      ],
      startimes: [
        { text: 'Startimes Nova', value: 'nova' },
        { text: 'Startimes Basic', value: 'basic' },
        { text: 'Startimes Smart', value: 'smart' },
        { text: 'Startimes Classic', value: 'classic' },
        { text: 'Startimes Super', value: 'super' },
      ],
    }
  },
  methods: {
    tryy() {
      alert(this.t)
    },
    getNetwork() {
      if (this.network == 'dstv') {
        this.dstv_ = true
        this.gotv_ = false
        this.startimes_ = false
      }

      if (this.network == 'gotv') {
        this.gotv_ = true
        this.dstv_ = false
        this.startimes_ = false
      }

      if (this.network == 'startimes') {
        this.startimes_ = true
        this.gotv_ = false
        this.dstv_ = false
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
        const res = await this.$axios.$get(`${this.$config.baseUrl}tv?`, {
          params: {
            username: this.$config.username,
            password: this.$config.password,
            phone: '08131668189',
            service_id: this.network,
            smartcard_number: this.smartCard,
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
