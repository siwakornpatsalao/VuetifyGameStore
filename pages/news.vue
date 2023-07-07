<template>
    <div>
        <div class="custom">
            <h2 class="textG" style="margin-bottom: -70px; margin-left: 26%; font-size: 150px;">Latest News</h2>
            <br><br>
            <br><br>
            <br><br>
            <br><br>
            <br><br>
        </div>
        <br>
        <news2></news2>
        <div v-for="data in dataPer" :key="data.id" style="margin-left:50px">
            <hr style="width: 90%;">
            <v-row >
              <a style="display: flex" :href="data.article_url">  
                <v-img style="margin-right:20px; margin-top:40px; margin-left: 20px;" :src="data.thumbnail" width="250" height="200"></v-img>
              </a>
              <v-col cols="7" style="margin-top:40px;">
                <v-card style="background-color: rgb(24, 23, 23);" :href="data.article_url"><!-- style="color:rgb(24, 23, 23);" -->
                <h4 style="color: #d1363a">Article</h4>
                <h3 style="margin-top:20px; font-size: 30px;">{{ data.title }}</h3>
                <p style="color: grey">{{ data.short_description }}</p>
                </v-card>
              </v-col>
            </v-row>
            <br><br>
            <br><br>
            <hr style="width: 90%;">
        </div>


        <div class="text-center">
            <v-pagination v-model="page" :length="Math.ceil(datasP.length/perPage)" @input="update" :total-visible="7">
            </v-pagination>
        </div>    
    </div>
</template>

<script lang="ts">
import Vue from 'vue'
import news2 from '~/components/news/news2-head.vue';

interface News{
    id: number;
    title: string;
    article_url: string;
    thumbnail: string;
    short_description: string;
}

interface GameData {
  id: number;
  thumbnail: string;
  title: string;
  release_date: string;
  genre: string;
  platform: string;
}

export default Vue.extend({
    components: {news2},
    data(){
        return{
            datas: [] as News[],
            datasP: [] as News[],
            datas1: [] as GameData[],
            perPage: 8,
            page: 1,
        }
    },
    async created(){
        const { data } = await this.$axios.get("http://localhost:9000/news");
        this.datas = data;
        console.log(data);
    },
    computed:{
        dataPer(){
            const start = (this.$data.page - 1) * this.$data.perPage;
            const end = start + this.$data.perPage;

            let temp = this.$data.datas;

            this.datasP = temp;
            return temp.slice(start,end);
        }
    },
    methods:{
        update(pages:number){
            this.page = pages;
        },
    }
})


</script>

<style scoped>
.custom{
 /* background: rgb(24, 23, 23 );*/
 background-image: url(https://wallpaper-mania.com/wp-content/uploads/2018/09/High_resolution_wallpaper_background_ID_77700340235.jpg);
 background-repeat: no-repeat;
 background-size: 100%;
}
.textG{
     text-shadow: 4px 4px 8px blue;
}
.v-card:hover h3 {
  color: red;
}
</style>