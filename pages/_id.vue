<template>
    <div style="margin-left: 100px">
        <v-row>
          <v-col cols="7">
            <br>
            <h2 style="font-size:32px">{{ datas[0].title }}</h2>
            <br>
        <v-img  width="700px" height="480px" :src="datas[0].thumbnail"></v-img>
        <v-col><span size="30px">{{ datas[0].short_description }}</span> </v-col>
        <v-col>
            <br><br>
            <v-card  width="780px">
                <div class="d-flex align-end justify-space-between" >
                <span style="font-size:20px">Buy {{ datas[0].title }}</span>
                <div class="text-right">
                    <v-btn :href="datas[0].game_url" color="#2e6930">Buy Now</v-btn>
                </div>
            </div>
            </v-card>
            <br>
            <v-card  width="780px">
                <div class="d-flex align-end justify-space-between" >
                <span style="font-size:20px">Buy {{ datas[0].title }} - Deluxe Edition</span>
                <div class="text-right">
                    <v-btn :href="datas[0].game_url" color="#2e6930">Buy Now</v-btn>
                </div>
            </div>
            </v-card>
        </v-col>
        <br><br>
        </v-col>
        <div>
        <v-col style="margin-top: 80px">
          <v-img :src="datas[0].thumbnail" height="151px" width="320px"></v-img>
          <br>
          <v-col><span style="color: gray;">Developer: </span><a>{{ datas[0].developer }}</a></v-col>
          <v-col><span style="color: gray;">Publisher: </span><a>{{ datas[0].publisher }}</a></v-col>
          <v-col><span style="color: gray;">Release Date: </span>{{ datas[0].release_date }}</v-col>
          <v-col><span style="color: gray;">Platform: </span><a>{{ datas[0].platform }}</a></v-col>
          <v-col><span style="color: gray;">Genre: </span> <a>{{ datas[0].genre }}</a></v-col>
        </v-col>

        <div style="margin-top:100px">
        <v-col style="margin-top: 50px">
          <h3>MORE LIKE THIS</h3>
          <br>
          <v-row>
            <v-col v-for="game in sameGen" :key="game.id" cols="2.5">
              <v-img @click="goTo(game)" :src="game.thumbnail" height="100px" width="150px"></v-img>
              <v-col><span>{{ game.title }}</span></v-col>
            </v-col>
          </v-row>
        </v-col>
        
      </div>
      </div>
        </v-row>
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
}

export default Vue.extend({  
    data(){
        return{
            datas: [] as GameData[],
            id: ''
        }
    },
    created(){
        this.id = this.$route.params.id;
    },
    async asyncData({$axios,params}){
        const { data } = await $axios.get("http://localhost:9000/games");
        const data2 = data.Games;
        const filteredData = data2.filter((key:GameData) => key.id === Number(params.id));

        const genre = filteredData[0].genre; 
        
        const relatedData = data2.filter((key:GameData) => key.genre === genre && key.id !== Number(params.id));
        const shuffle = relatedData.sort(() => Math.random() - 0.5);
        const randomData = shuffle.slice(0, 3);
        return { datas: filteredData, sameGen: randomData  };
    },
    methods:{
        goTo(game:GameData){
            this.$router.push(`/${game.id}`);
        },
    }
})

</script>
