<template>
  <div class="wrap">
    <Header title="Simon The Game"/>
    <Content/>
    <Footer copyright="Copyright 2020"/> 
  </div>  
</template>

<script>
import Header from "./Header.vue"
import Content from "./content/Content.vue"
import Footer from "./Footer.vue"
export default {
  components:{
    Header,
    Content,
    Footer
  }
}
</script>

<style lang="scss" scoped>

</style>
