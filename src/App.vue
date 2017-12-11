<template>
  <div id="app">
<lefts></lefts>
    <div class="main">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
  import lefts from '@/components/common/left'
export default {
  name: 'app',
  components:{
    lefts
  }
}
</script>

<style>
  #app{
    overflow: hidden;
  }
.main{
  float: left;
  width: 95%;
  height: 100%;
  background-color: beige;
}
</style>
