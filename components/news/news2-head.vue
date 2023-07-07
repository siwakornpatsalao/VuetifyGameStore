<template>
    <div>
        <v-row>
        <v-col v-for="data in news" :key="data.id" cols="6" >
            <v-img class="img" style="margin-top:20px; border-radius: 2.5%;" cover height="500" width="1500" @click="goURL(data.article_url)" :src="data.main_image">
            <div style="margin-top: 400px; margin-left: 20px;"> 
                <h3 class="text1">{{ data.title }}</h3>
            </div>
            </v-img>
        </v-col>
        </v-row>

        <v-row>
        <v-col v-for="data in news2" :key="data.id" >
            <div>
            <v-img class="img" height="200" width="450" :src="data.thumbnail"  @click="goURL(data.article_url)"></v-img>
            <p>{{ data.title }}</p>
            </div>
        </v-col>
        </v-row>

    </div>
</template>

<script lang="ts">
import Vue from 'vue'

interface News{
    id: number;
    title: string;
    article_url: string;
    thumbnail: string;
    short_description: string;
    main_image: string
}

export default Vue.extend({
    data(){
        return{
            news: [] as News[],
            news2: [] as News[],
        }
    },
    async created(){
        const { data } = await this.$axios.get("http://localhost:9000/news");
        const data1 = data.slice(0,2);
        const data2 = data.slice(2,6);
        this.news = data1;
        this.news2 = data2;
    },
    methods:{
        goURL(data:string){
            window.location.href = data;
        }
    }
})

</script>

<style scoped>
.img{
    cursor: default;
}
.img:hover{
    cursor: pointer;
}
.text1{
    font-size: 31px;
}
</style>