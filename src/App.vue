<script setup lang="ts">
import NavigationHeader from "./components/NavigationHeader.vue"
import Footer from "./components/Footer.vue";


</script>

<template>
  <el-affix :offset="0">
    <NavigationHeader></NavigationHeader>
  </el-affix>
  <RouterView></RouterView>
    <Footer class="footer"></Footer>
</template>

<style>
   @font-face {
    font-family: 'Lora';
    src: url('https://fonts.googleapis.com/css?family=Lora&display=swap');
    
   }



   #footer{
    position:fixed;
    bottom:0;
}

#app {
    min-height: 100vh; 
    margin: 0; 
    display: grid;
    grid-template-rows: 1fr auto;
    font-family: 'Lora';
    src: url('https://fonts.googleapis.com/css?family=Lora&display=swap');
    
}

</style>
