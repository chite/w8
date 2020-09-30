<template>
  <!-- <div id="nav"> -->
  <!-- <router-link to="/">Home</router-link> | -->
  <!-- <router-link to="/about">About</router-link> -->
  <!-- </div> -->
  <!-- <router-view /> -->
  <home></home>
</template>
<script>
import Home from '@/views/Home.vue'
export default {
  components:{
    Home
  },
  mounted(){
    if (document.getElementById('fontID')) return; // was already loaded
      let tag = document.createElement("link");
      tag.crossorigin="anonymous"
      tag.rel = "stylesheet";
      tag.href ="https://pro.fontawesome.com/releases/v5.10.0/css/all.css" 
      tag.id = "fontID";
      document.getElementsByTagName('head')[0].appendChild(tag);
  }
}
</script>
<style>
html{
  font-size: 14px;
}
body {
  padding: 0;
  margin: 0;
}
</style>
