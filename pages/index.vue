<template>
    <div>
    <heads></heads>
    <gameIndex></gameIndex>
    <news></news>
    </div>
</template>

<script lang="ts">
import heads from '@/components/indexPage/heads.vue';
import news from '@/components/indexPage/news2.vue';
import gameIndex from '~/components/indexPage/slideGroup.vue';

export default{
  name: "index",
  components: {heads,news,gameIndex}
}
</script>

<style scoped>

</style>