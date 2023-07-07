<template>
  <div>
      <v-slide-group>
            <v-slide-item v-for="data in datas" :key="data.id" class="ma-5">
              <v-card class="head" color="#20243c" elevation="10" max-width="300px" max-height="500px">
                <v-img class="thisYear2"
                  :src="data.thumbnail"
                  height="400px"
                ></v-img>
                <v-card-text color="#20243c">
                  <span style="color: #20243c">akdfgjdflkajgldfjglkjdlkajfglfjgkdlfgjlkdjsl</span>
                </v-card-text>
                <v-card-actions>
                  <v-spacer/>
                </v-card-actions>
              </v-card>
            </v-slide-item>
          </v-slide-group>
  </div>
</template>

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
  name: "HeadGame",
  data(){
      return{
          datas: [] as GameData[],
          datas2: [] as GameData[],
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
      goTo(data:GameData){
          this.$router.push(`/${data.id}`);
      }
  }
})
</script>

<style scoped>
@import '@/assets/head.css';
@import '@/assets/tag.css';
</style>