<template>
    <v-main class="custom">
    <div>
        <div style="display: flex">
            <h2 style="font-size: 48px; margin-left: 20px;">Games</h2>
            <v-btn style="margin-left:1500px; margin-top: 30px;" @click="goTo">
            Show More
        </v-btn>
        </div>
        <v-slide-group center-active >
              <v-slide-item v-for="data in datas" :key="data.id" class="ma-5">
                <v-card style="border-radius: 5%;" class="head2" color="#20243c" max-width="361px" max-height="600">
                  <v-img style="border-radius: 5%; margin-top: 5px; margin-left: 5px;"
                    cover
                    :src="data.thumbnail"
                    height="400px"
                    width="350px"
                  >
                    <v-icon v-if="data.platform== 'PC (Windows)'" class="icon1">mdi-monitor</v-icon>
                    <v-icon v-if="data.platform== 'Web Browser'" class="icon1">mdi-web-box</v-icon>
                    <v-icon v-if="data.platform !='PC (Windows)' && data.platform != 'Web Browser'" class="icon1">mdi-controller</v-icon>
                  </v-img>
                  <v-card-text class="text" color="#20243c">
                    <span style="font-size: 14px">{{ data.title }}</span>
                    <br><br>
                    <span style="font-size: 12px">{{data.short_description}}</span>
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer/>
                    <v-btn color="white" @click="goToId(data)">
                      <span style="color: black">
                        learn more
                      </span>
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-slide-item>
            </v-slide-group>
    </div>
    </v-main>
</template>
  <!-- <v-btn style="margin-left:1500px; margin-top:40px" @click="goTo">
    Show More
</v-btn> -->

<script lang="ts">
import Vue from 'vue'
interface GameData {
  id: number;
  thumbnail: string;
  title: string;
  release_date: string;
  genre: string;
  platform: string;
  url : string;
  publisher: string;
  developer: string;
  short_description: string;
  date: string;
}

export default Vue.extend({
    data(){
        return{
            datas: [] as GameData[],
        }
    },
    async created(){
        try {
          const { data } = await this.$axios.get("http://localhost:9000/games");
          data.Games.sort((a:GameData, b:GameData) => Date.parse(b.release_date) - Date.parse(a.release_date));
          this.datas = data.Games.slice(0,10);
        }catch (error) {
            console.error(error);
        } 
    },
    methods:{
        goTo(){
            this.$router.push(`/game`);
        },
        goToId(data:GameData){
            this.$router.push(`/${data.id}`);
        }
    }
})
</script>

<style scoped>
@import '@/assets/head.css';
@import '@/assets/tag.css';
.custom{
    background: linear-gradient(to bottom right, #641513, black);
}
</style>