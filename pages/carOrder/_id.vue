<template>
    <v-row>
    <v-col cols="12" sm="12">
      <v-card rounded="lg" min-height="50">
        <h1 class="text-center">รายละเอียดการจอง</h1>
      </v-card>
    </v-col>
    <v-col cols="12" sm="6">
      <v-card rounded="lg" min-height="400"  
        ><v-img
          class="white--text align-end"
          height="400px"
          :src="imgUrl"
          align="center"
        >
        </v-img
      ></v-card>
    </v-col>
    <v-col cols="12" sm="6"
      ><v-card rounded="lg" min-height="400" color="indigo accent-1">
        <v-toolbar color="indigo accent-3" dark dense flat>
          <v-toolbar-title class="body-2"> {{ name }} </v-toolbar-title>
        </v-toolbar>
        <v-card-subtitle>{{ carID }}</v-card-subtitle>
        <v-card-text class="text--primary">
          <div class="dt">
            <v-icon class="ic" x-large> mdi-bitcoin </v-icon> ราคาเช่า 
            {{ price }} บาท/วัน
          </div>
          <div class="dt">
            <v-icon class="ic" x-large> mdi-car-seat </v-icon>
            {{ detail1 }} 
          </div>
          <div class="dt">
            <v-icon class="ic" x-large> mdi-treasure-chest </v-icon>
            {{ detail2 }}
          </div>
          <div class="dt">
            <v-icon class="ic" x-large>mdi-car</v-icon> 
            {{ status }}
          </div>
           </v-card-text>
           <v-col cols="6" sm="4">
        <v-menu
          ref="menu"
          v-model="menu2"
          :dateout-on-content-click="false"
          :nudge-right="40"
          :return-value.sync="DateStart"
          transition="scale-transition"
          offset-y
          max-width="100px"
          min-width="290px"
        >
          <template #activator="{ on, attrs }">
            <v-text-field
              v-model="DateStart"
              label="วันที่"
              prepend-icon="mdi-table-large"
              readonly
              required
              v-bind="attrs"
              v-on="on"
            ></v-text-field>
          </template>
          <v-date-picker
            v-if="menu2"
            v-model="DateStart"
            full-width
            @click:date="$refs.menu.save(DateStart)"
          ></v-date-picker>
        </v-menu>
      </v-col>

      <v-col cols="6" sm="4">
        <v-menu
          ref="menu1"
          v-model="menu1"
          :dateout-on-content-click="false"
          :nudge-right="40"
          :return-value.sync="DateEnd"
          transition="scale-transition"
          offset-y
          max-width="290px"
          min-width="290px"
        >
          <template #activator="{ on, attrs }">
            <v-text-field
              v-model="DateEnd"
              label="ถึงวันที่"
              prepend-icon="mdi-table-large"
              readonly
              required
              v-bind="attrs"
              v-on="on"
            ></v-text-field>
          </template>
          <v-date-picker
            v-if="menu1"
            v-model="DateEnd"
            full-width
            @click:date="$refs.menu1.save(DateEnd)"
          ></v-date-picker>
        </v-menu>
      </v-col>
      <div class="dt">
            เช่าจำนวน -- {{ day }} -- วัน
      </div>
      </v-card>
    </v-col>
    
  <v-container>
        <h1>ชำระเงิน</h1>
        <h1>{{ radios || 'null' }}</h1>

        <v-radio-group v-model="radios" mandatory
          ><div class="text-right">
          </div>
          <div class="text">
            <v-row>
              <v-col cols="10" sm="6">
                <v-radio
                  label="Select to promptpay"
                  value="Select to promptpay"
                >
                </v-radio
                ><img src="@/pages/QR.jpg" height="max" width="max" /></v-col
            ></v-row>
          </div>
        </v-radio-group>

<div class="text-right">
      
      <br /><br />
      <v-card-text v-model="Total"><h1>ยอดรวม : -- {{ price * day }} -- บาท</h1> </v-card-text>
    </div>
    <!-- <div class="text-right">
      <v-btn color="bulbul" @click="addData"> ชำระเงิน </v-btn>
    </div>   -->

<div class="text-center">
    <v-btn
      dark
      color="orange darken-2"
      v-on:click="addData"
      @click="snackbar = true "
    >
      ชำระเงิน
    </v-btn>

    <v-snackbar
      v-model="snackbar"
      :timeout="timeout"
    >
      {{ text }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="blue"
          text
          v-bind="attrs"
          @click="flat"
          :to="`/ShopEmployee`"
        >
          ตกลง
        </v-btn>
      </template>
    </v-snackbar>

</div>

      </v-container>
      
  </v-row>
  
</template>

<script>
import { db } from '~/plugins/firebaseConfig.js'
import 'vue-hotel-datepicker/dist/vueHotelDatepicker.css';
export default {
  data() {
    return {
      list: [],
      carid: this.$route.params.id.carID,
      name: this.$route.params.id.name,
      price: this.$route.params.id.price,
      imgUrl: this.$route.params.id.imgUrl,
      detail1: this.$route.params.id.detail1,
      detail2: this.$route.params.id.detail2,
      status: this.$route.params.id.status,
      DateStart: '',
      DateEnd: '',
      radios: '',
      menu1: false,
      menu2: false,
      sum: '',
      nameRules: [(v) => !!v || 'please required'],
      data: [],
      snackbar: false,
      text: 'คุณสั่งจองสำเร็จ รอยืนยันสถานะ ',
      timeout: 2000,
      

    }
  },
  computed: {

    day() {
    const DateStart = new Date(this.DateStart)
    const DateEnd = new Date(this.DateEnd)
    const age = Math.abs(
      Math.floor((DateEnd - DateStart) / (24 * 60 * 60 * 1000)) + 1
    )
    return age
    },


  },
  methods: {
    set() {
      const day = Math.abs(this.DateStart, this.DateEnd)
      this.sum = day / (1000 & 60 * 60)
      console.log('DAY' + this.day)
    },
    addData() {
      const data = {
        DateStart: this.DateStart,
        DateEnd: this.DateEnd,
        day: this.day,
        carName: this.name,
        Total: this.price * this.day
      }
      db.collection('DBorders')
        .doc()
        .set(data)
        .then(function () {
          console.log('Document successfully written! -> MyForm')
        })
        .catch(function (error) {
          console.error('Error writing document: ', error)
        })
    },
    getdata() {
      db.collection('Employee')
        .orderBy('mail', 'asc')
        .onSnapshot((querySnapshot) => {
          const data = []
          querySnapshot.forEach((doc) => {
            data.push(doc.data())
          })
          this.list = data
          console.log(this.list)
        })
    },
  },


}



</script>
<style>
.ic {
  margin-right: 2rem;
}
.dt {
  margin: 1rem;
}
</style>