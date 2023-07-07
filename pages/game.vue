<template>
  <v-main class="custom">
  <div>
    <!-- <br>
     <head-game></head-game> 
    <head-game></head-game>
    <br><br>
    <hr/>
    <br><br> -->
    <h1 class="textG" style="margin-bottom: -50px; margin-left: 29%; font-size: 150px;">GAME LIST</h1>
    <br><br>
    <br><br>
    <br><br>
    <br><br>
    <br><br>
    <list-game></list-game>
  </div>
</v-main>
</template>

<script lang="ts">
import HeadGame from '@/components/gameList/headGame.vue';
import ListGame from '@/components/gameList/ListGame.vue';
export default{
  name: "index",
  components: {ListGame,HeadGame}
}
</script>

<style scoped>
@import '@/assets/head.css';
@import '@/assets/frame.css';
.custom{
 /* background: rgb(24, 23, 23 );*/
 background-image: url(https://i.stack.imgur.com/SvWWN.png);
 background-repeat: no-repeat;
 background-size: 100%;
}
.textG{
  text-shadow: 4px 4px 8px red;
}
</style>