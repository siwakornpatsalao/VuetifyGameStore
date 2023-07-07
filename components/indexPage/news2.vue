<template>
    <v-main class="custom">
    <div style="margin-left:20px">
            <h2 style="font-size:48px">What's Happening</h2>
            <v-btn style="margin-left:1500px;" @click="goTo">
                    Show More
            </v-btn>
        <br>
        <v-row>
        <v-col v-for="data in news2" :key="data.id" cols="6">
            <v-img class="img" style="margin-top:20px; border-radius: 2.5%;" cover height="500" width="1500" @click="goURL(data.article_url)" :src="data.main_image">
                <div style="margin-top: 400px; margin-left: 20px;"> 
                    <h3 class="text1">{{ data.title }}</h3>
                </div>
            </v-img>   
        </v-col>
        
        <v-col cols="6">
        <v-col v-for="data in news" :key="data.id" >
            <v-col>
                <v-card :href="data.article_url" rounded="xl" class="card" width="700" height="120">
                    <div style="display: flex">
                    <h3 class="text2">{{ data.title }} </h3>
                    <v-img style="margin-left:25px; border-radius: 5%;" width="200px" height="120px" :src="data.thumbnail"></v-img>  
                    </div>
                </v-card>
            </v-col>
        </v-col>
        </v-col>
    </v-row>
    <br>
    <br>
    </div>
</v-main>
</template>

<script lang="ts">
import Vue from 'vue';
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
        const data1 = data.slice(1,5);
        const data2 = data.slice(0,1);
        this.news = data1;
        this.news2 = data2;
        console.log(data1);
    },
    methods:{
        goTo(){
            this.$router.push(`/news`);
        },
        goURL(data:string){
            window.location.href = data;
        }
    }
})

</script>

<style scoped>
.text1{
    font-size: 31px;
}
.text2{
    font-size:18px;
    margin-top:25px;
    margin-left:10px;
}
.custom{
    background: black;
}

.card{
    margin-bottom: -35px;
}

.img{
    cursor: default;
}
.img:hover{
    cursor: pointer;
}
</style>