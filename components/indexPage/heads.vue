<template>
    <v-img height="900" width="100vw" src="https://wallpapers.com/images/featured/305kescxw5dpup7y.jpg">
    <!-- <v-img height="717" width="100vw" src="https://64.media.tumblr.com/8421c7236715e6c9a93fbcd2f359096f/0c1f12382877670a-7c/s540x810/4e9c4e751f5a0722e1a7f6ae085c90f53c1a0517.gif"> -->
    <div style="margin-left:40px">
      <v-img style="margin-top:280px"  aspect-ratio="16-9" height="298" width="450" src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Valorant_logo_-_pink_color_version.svg/2560px-Valorant_logo_-_pink_color_version.svg.png"></v-img>
      <v-btn class="btn" width="250" height="72" color="#d1363a" href="https://playvalorant.com/th-th/?gad=1&gclid=Cj0KCQjw4NujBhC5ARIsAF4Iv6eK_bkofcsm-dioX19jM1og0dDacIDcN0NoMkO9aZkE9sj0NY4AiMkaAv4MEALw_wcB&gclsrc=aw.ds">
        <v-icon size="50px">mdi-arrow-right-bold-circle</v-icon>
        <span class="size">Learn More</span></v-btn> 
    </div></v-img>
</template>
  

<script lang="ts">
import Vue from 'vue';
export default Vue.extend({

})
</script>

<style scoped>
.size{
    font-size: large;
}
.btn{
    margin-top: 20px;
    margin-left: 100px;
    transition: background-color 0.3s ease;
}
.btn:hover{
    color: black;
}
</style>