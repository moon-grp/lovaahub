<template>
  <div id="bg">
    <div>
      <v-row>
        <v-col>
          <v-card color="#252362" dark class="" width="100%" height="150">
            <v-card-title class="text-h6 text-capitalize"
              >Current Balance
              <v-icon class="ml-4" left>
                mdi-cash-multiple
              </v-icon></v-card-title
            >

            <v-card-actions>
              <v-btn text class="overline"> {{ balance }}</v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </div>

    <!-- mobile version -->
  </div>
</template>

<script>
export default {
  layout: 'dashboard',
  data() {
    return {
      balance: '',
    }
  },
  methods: {
    async getBalance() {
      try {
        const res = await this.$axios.$get(`${this.$config.baseUrl}balance?`, {
          params: {
            username: this.$config.username,
            password: this.$config.password,
          },
        })
        this.balance = res.data.currency + ' ' + res.data.balance

        console.log(res)
      } catch (error) {
        console.log(error.response)
      }
    },
  },
  created() {
    this.getBalance()
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
  font-size: 52px;
  font-weight: bold;
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

.v-data-table {
  width: 100%;
}
</style>
