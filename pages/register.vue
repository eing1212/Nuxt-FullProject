<template>
  <div class="login100">
    <v-card class="wrap-login100" color="rgba(255, 255, 255, 0.75)">
      <v-row class="center"
        ><v-col cols="6">
          <v-img src="https://illustoon.com/photo/1774.png"></v-img
        ></v-col>
        <v-col cols="6">
          <v-container>
            <v-form>
              <v-row>
                <v-col cols="12">
                  <h1 class="logtitle">Sign up</h1>
                  <v-text-field
                    v-model="Firstname"
                    label="First Name"
                    required
                    filled
                    rounded
                    dense
                    solo-inverted
                  ></v-text-field
                >
                <v-text-field
                    v-model="Lastname"
                    label="Lastname"
                    required
                    filled
                    rounded
                    dense
                    solo-inverted
                  ></v-text-field
                >
                <v-text-field
                    v-model="Tel"
                    label="Tel"
                    required
                    filled
                    rounded
                    dense
                    solo-inverted
                  ></v-text-field
                >
                <v-text-field
                    v-model="numberID"
                    label="PassPort ID"
                    required
                    filled
                    rounded
                    dense
                    solo-inverted
                  ></v-text-field
                >
                  <v-text-field
                    v-model="email"
                    label="E-mail"
                    required
                    filled
                    rounded
                    dense
                    solo-inverted
                    :rules="emailRules"
                  ></v-text-field
                ></v-col>
                <v-col cols="12">
                  <v-text-field
                    v-model="password"
                    :append-icon="show2 ? 'mdi-eye' : 'mdi-eye-off'"
                    :type="show2 ? 'text' : 'password'"
                    :rules="[rules.required, rules.min]"
                    name="input-10-2"
                    label="Password"
                    class="input-group--focused"
                    filled
                    rounded
                    dense
                    solo-inverted
                    @click:append="show2 = !show2"
                  ></v-text-field
                ></v-col>
                <!-- <v-col cols="12">
                  <v-text-field
                    v-model="confirmpassword"
                    :append-icon="show2 ? 'mdi-eye' : 'mdi-eye-off'"
                    :type="show2 ? 'text' : 'password'"
                    name="input-10-2"
                    label="Confirm Password"
                    class="input-group--focused"
                    filled
                    rounded
                    dense
                    solo-inverted
                    :rules="[comparepasswords]"
                    @click:append="show2 = !show2"
                    required
                  ></v-text-field></v-col
              > -->
              </v-row>
              <v-hover>
                <v-btn
                  rounded
                  class="container-btn"
                  color="#DCB13C"
                  @click="signUp()"
                  v-on:click="confirm"
                >
                  SIGN UP
                </v-btn></v-hover>
            </v-form>
          </v-container>
        </v-col>
      </v-row>
    </v-card>
  </div>
</template>

<script>
import firebase from 'firebase/app'
import { db } from '~/plugins/firebaseConfig.js'
import { auth } from '~/plugins/firebaseConfig.js'
export default {
  layout: 'login',
  data() {
    return {
      show2: false,
      Firstname:'',
      Lastname:'',
      Tel:'',
      numberID:'',
      email: '',
      password: '',
      clock: {
        el: '#clock',
        data: {
          time: '',
          date: '',
          signUp: '',
        },
      },

      emailRules: [
      v => !!v || 'E-mail is required',
      v => /.+@.+\..+/.test(v) || 'E-mail must be valid'
      ],
      show1: false,
      rules: {
      required: value => !!value || 'Required.',
      min: v => v.length >= 8 || 'Min 8 characters'
      },
    }
  },
//   computed: {
//     comparepassword() {
//       return this.password !== this.confirmpassword
//         ? 'Password do not match'
//         : ''
//     },
//   },
  methods: {
    signUp() {
      auth
        .createUserWithEmailAndPassword(this.email, this.password)
        .catch(function (error) {
          // Handle Errors here.
          var errorCode = error.code
          var errorMessage = error.message
          if (errorCode == 'auth/weak-password') {
            alert('The password is too weak.')
          } else {
            alert(errorMessage)
          }
          console.log(error)
        })
    },
    checkUser() {
      var user = auth.currentUser
      console.log(user === null)
    },

    confirm() {
        const data = {
        Firstname : this.Firstname,
        Lastname : this.Lastname,
        Tel : this.Tel,
        numberID : this.numberID
        }
        db.collection('DBEmployee')
        .doc()
        .set(data)
        .then(function () {
          console.log('Document successfully written! -> MyForm')
        })
        .catch(function (error) {
          console.error('Error writing document: ', error)
        })
    }
    }
  }
</script>
<style>
.sp {
  margin-top: 1rem;
}
.container-btn {
  width: 100%;
  display: -webkit-box;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  padding-top: 10px;
}
.logtitle {
  color: #333333;
  line-height: 1.2;
  text-align: center;
  width: 100%;
  display: block;
  padding-bottom: 54px;
}
.wrap-login100 {
  width: 960px;
  border-radius: 20px;
  overflow: hidden;
  display: -webkit-box;
  display: -webkit-flex;
  display: -moz-box;
  display: -ms-flexbox;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  padding: 50px 50px 50px 95px;
}
.login100 {
  width: 100%;
  min-height: 100vh;
  display: -webkit-box;
  display: -webkit-flex;
  display: -moz-box;
  display: -ms-flexbox;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background: -webkit-linear-gradient(-135deg, #c48313, #01bcc6);
  background: -o-linear-gradient(-135deg, #ac7311, #965334);
  background: -moz-linear-gradient(-135deg, #080808, #008eab);
  background: linear-gradient(-135deg, #000000, #008eab);
}
</style>
