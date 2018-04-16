<template>
  <div id="app">  
    <leftnav></leftnav>
    <div class="main">
    <router-view></router-view>
    </div>
  </div>
</template>

<script>
import leftnav from '@/components/common/leftNavbar'
export default {
  name: 'App',
  components:{
  	leftnav
  }
}
</script>

<style>
#app {
  font-family:"Microsoft Yahei",'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100%;
}
.main{
  float:left;
  width:95%; 
  background-color: #EFF2F7;
  height:100%;
  overflow: auto;
}
</style>
