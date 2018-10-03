<template>
  <div id="app">
    <Header></Header>
    <div class="main">
    <router-view name="slidebar"></router-view>
    <router-view name="main"></router-view>
    </div>
  </div>
</template>

<script>
import Header from'./components/Header'
import postList from'./components/PostList'

export default {
  name: 'App',
  components:{
    Header,postList
  }
}
</script>

<style>
@import url('./assets/markdown-github.css');
.main{
  margin: 15px auto;
  width: 90%;
}

</style>
