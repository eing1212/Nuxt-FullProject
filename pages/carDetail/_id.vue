<template>
        <v-row>
    <v-col cols="12" sm="12">
      <v-card rounded="lg" min-height="50">
        <h1 class="text-center">รายละเอียดรถยนต์</h1>
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
          </div> </v-card-text>
      </v-card>
    </v-col>

    <v-col cols="12" sm="12"> </v-col>
  </v-row>
    
</template>

<script>
import { db } from '~/plugins/firebaseConfig.js'
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
    }
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