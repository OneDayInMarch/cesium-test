<template>
  <div id="cesiumContainer"></div>
</template>
 
<script setup>
import * as Cesium from 'cesium';
import { onMounted } from 'vue';
onMounted(() => {
  const viewer = new Cesium.Viewer('cesiumContainer',{
      infoBox: false, // 禁用沙箱，解决控制台报错
    });
});
</script>
 
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
html,body,#cesiumContainer{
  width: 100%;
  height: 100%;
  padding: 0;
  margin: 0;
  overflow: hidden;
}
</style>