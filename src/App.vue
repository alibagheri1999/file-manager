<template>
  <Header />
  <div class="body">
    <Home />
    <Content />
  </div>
</template>

<script>
import Home from './components/Home.vue'
import Header from './components/Header.vue'
import Content from './components/Content.vue'

export default {
  name: 'App',
  components: {
    Home,
    Header,
    Content
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /*margin-top: 60px;*/
}
body{
  background-color: #02001f;
  color: yellow;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  margin: 0;
  padding: 0;
}
.body{
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}
</style>
