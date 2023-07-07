<template>
    <v-main class="background">
    <div style="margin-left: 100px">
        <v-row>
            <v-col cols="9">
                <div>
                    <v-select v-model="sort"
                      @change="sorted"
                      label="Sort"
                      style="max-width: 150px;"
                      :items="['Alphabetic','Release Date']">
                    </v-select>
                </div>
                <div class="container">
                    <v-container>
                        <v-row>
                            <v-col v-for="data in filterList" :key="data.id">
                                <v-card class="frame" width="300" style="border-radius: 5%; justify-content: center;"
                                color="#1f1f1f"
                                @click="goTo(data)">
                                    <v-img  style="margin-top:5px; margin-left: 15px; border-radius: 5%;"  width="270.25" height="270.66" :src="data.thumbnail">
                                        <v-card width="65px" height="30px"
                                                :class="{'blue1' : data.platform == 'PC (Windows)',
                                                'red1' : data.platform == 'Web Browser', 
                                                'green1': data.platform !='PC (Windows)' && data.platform != 'Web Browser'}">
                                    <p>{{ data.platform == 'PC (Windows)' ? 'PC' : (data.platform == 'Web Browser' ? 'Web' : 'WB&PC') }}</p>
                                        </v-card>
                                        <v-icon v-if="data.platform== 'PC (Windows)'" class="icon1">mdi-monitor</v-icon>
                                        <v-icon v-if="data.platform== 'Web Browser'" class="icon1">mdi-web-box</v-icon>
                                        <v-icon v-if="data.platform !='PC (Windows)' && data.platform != 'Web Browser'" class="icon1">mdi-controller</v-icon>
                                        <v-card v-if="new Date(data.release_date).getFullYear() === new Date().getFullYear()" class="thisYear" height="20px" width="85px">
                                        <p>New</p>
                                        </v-card>
                                    </v-img>
                                    <p style="margin-top: 30px; margin-left: 10px; color: skyblue">{{ data.genre }}</p>
                                    <p style="margin-top: 20px; margin-left: 10px;">{{ data.title }}</p>
                                    <br>
                                </v-card>
                            </v-col>
                        </v-row>
                    </v-container>
                </div>
            </v-col>

            <v-col>
                <div style="margin-left: 20px">
                <div style="margin-top: 85px" >
                <v-btn style="margin-bottom: 10px; margin-left: 50px;" @click="reset">Reset</v-btn>
                <v-text-field v-model="search" 
                    prepend-inner-icon="mdi-magnify" 
                    outlined
                    placeholder="Keywords"
                    style="max-width: 200px; margin-left: 50px;"
                    >
            </v-text-field>
                </div>
                <!-- <v-select v-model="sort"
                      @change="sorted"
                      label="Sort"
                      style="max-width: 200px; margin-left: 50px;"
                      :items="['Alphabetic','Release Date']">
                    </v-select>  -->
                <v-select v-model="genre" label="Genre" :items="genres" style="max-width: 200px; margin-left: 50px; "></v-select>
                <v-select v-model="platform" label="Platform" :items="platforms" style="max-width: 200px; margin-left: 50px; "></v-select>
                
                </div>

                <!-- div style="margin-top: 20px;">
            <h3>FEATURE GAME</h3>
            <br>
                <p v-for="game in filterGame" :key="game.id">
                    <v-img @click="goTo(game)" :src="game.thumbnail" height="150px" width="250px"></v-img>
                    {{ game.title }}
                    <br>
                </p>
        </div> -->
            </v-col>

        </v-row>

        <div class="text-center">
    <v-pagination v-model="page" :length="Math.ceil(filter.length/perPage)" @input="update" :total-visible="7">
    </v-pagination>
        </div>
    </div>
</v-main>
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
    name: "ListGame",
    data(){
        return{
            sort: null,
            datas: [] as GameData[],
            search: "",
            genre: "",
            platform: "",
            page: 1,
            perPage: 32,
            filter: [] as GameData[],
            genres: [],
            platforms: [],
        }
    },
    async created(){
        try {
            const { data } = await this.$axios.get("http://localhost:9000/games");
            //const { data } = await $axios.get("/api/api1/games");
            data.Games.sort((a:GameData, b:GameData) => Date.parse(b.release_date) - Date.parse(a.release_date));
            this.datas = data.Games;
            this.genres = data.genres;
            this.platforms = data.platforms;
            console.log(data+"hey");
        } catch (error) {
            console.error(error);
        }
    },
    computed:{
        filterList(){
            const start = (this.$data.page - 1) * this.$data.perPage;
            const end = start + this.$data.perPage;

            let filteredData = this.$data.datas.filter((data:GameData) => {
                return data.title.toLowerCase().includes(this.search.toLowerCase());
            });

            if (this.genre) {
                filteredData = filteredData.filter((data:GameData) => {
                return data.genre === this.genre;
                });
            }

            if (this.platform) {
                filteredData = filteredData.filter((data:GameData) => {
                return data.platform === this.platform;
                });
            }
            this.filter = filteredData;
      
            return filteredData.slice(start,end);
        },
        filterGame(){
            let filterData = this.$data.datas;
            const filterData2 = filterData.sort(() => Math.random() - 0.5);
            return filterData2.slice(0,5)
        },
    },
    methods:{
        sorted(){
            if(this.sort == 'Alphabetic'){
            return this.datas.sort((a, b) => {
                if (a.title < b.title) return -1; // a first
                if (a.title > b.title) return 1; // b first
                return 0;
                });
            }else if(this.sort == 'Release Date'){
                return this.datas.sort((a, b) => {
                return new Date(a.release_date).getTime() - new Date(b.release_date).getTime();
                });  
            }else{
                return this.datas;
            }
        },
        reset(){
            //this.datas.sort((a:GameData, b:GameData) => a.id - b.id);
            this.datas.sort((a:GameData, b:GameData) => Date.parse(b.release_date) - Date.parse(a.release_date));
            this.sort = null;
            this.genre = "";
            this.platform = "";
            this.search = "";
        },
        goTo(data:GameData){
            this.$router.push(`/${data.id}`);
        },
        update(pages:number){
            this.page = pages;
        },
    }
}) 
</script>

<style scoped>
@import '@/assets/tag.css';
@import '@/assets/frame.css';
.background {
  background-color: rgb(24, 23, 23);
  padding: 10px;
  border-radius: 5px;
}
</style>