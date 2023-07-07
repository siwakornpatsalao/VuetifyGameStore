<template>
    <div style="margin-top: 170px ">
        <v-row style="margin-left:300px">     
        <v-img src="https://cdn.wallpapersafari.com/65/60/2aFVt7.jpg" height="600" width="0"></v-img>
        <!-- <v-img src="https://img.freepik.com/premium-vector/video-game-shop-logo_1056-48.jpg" height="500" width="0"></v-img> -->
        <v-card style="margin-right: 370px" width="50%">
        <v-sheet style="margin-top: 45px" width="400" height="380px" class="mx-auto" >
            
            <v-form fast-fail @submit.prevent="login(username)" v-model="isLoginValid">
                <v-row >
                <v-col style="margin-top: 30px">
                <v-card-text style="display: flex; align-items: center; margin-bottom: 20px;">
                    <v-icon size="50px" style="margin-right: 8px;">mdi-account </v-icon> <h1>LOGIN</h1>
                </v-card-text>
                <v-text-field variant="outlined" outlined v-model="username" label="Username"
                :rules="[() => !!username || 'Username Required']"
                ></v-text-field>

                <v-text-field variant="outlined" outlined v-model="password" label="Password"
                :rules="[() => !!password || 'Password Required']"
                @click:append="show = !show"
                :type="show ? 'text' : 'password'"
                ></v-text-field>
                <a href="#" class="text-body-2 font-weight-regular">Forgot Password?</a>
                <br><br>

           <v-btn :disabled="!isLoginValid" type="submit" color="primary" block class="mt-2"
                  > Sign in </v-btn>
        </v-col>
    </v-row>
            </v-form>
            <div class="mt-2">
                <p class="text-body-2">Don't have an account? <a @click="goSign">Sign Up</a></p>
            </div>
        </v-sheet>
    </v-card>
</v-row>
    </div>
</template>

<script lang="ts">
import Vue from 'vue'

interface UserData {
  _id: string;
  username: string;
  gmail: string;
  password: string;
  name: string;
  city: string;
  surname: string;
  ageCal: number;
}

export default Vue.extend({
    /* layout: 'blank', */
    name: "Login",
    data(){
        return{
            isLoginValid: false,
            username: "",
            password: "",
            show: false,
            datas: [] as UserData[],
        }
    },
    methods:{
        async login(username: string) {
            const Swal = require('sweetalert2');
            console.log(this.password);
            try {
                const response = await this.$axios.post("http://localhost:9000/users/login", {
                username: this.username, password:this.password
                });
                const userData = response.data;

                if (userData.success) {
                localStorage.setItem("username",userData.username);
                this.$router.push({ name: "index" });
                }
            } catch (error:any) {
                Swal.fire({
                title: "<strong>Login Failed</strong>",
                text: error.response.data.errors[0].msg,
                icon: "error",
                });
            }
        },
        goSign(){
            this.$router.push({name:"register"});
        }
    }
})

</script>