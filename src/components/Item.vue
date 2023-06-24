<template>
  <div class="folders" ref="folders">
    <img class="images" :src="this.File.isExtended ? '/img/minus.svg': '/img/plus.svg'" alt="plus image" @click="toggle">
    <img class="images" :src="this.File.type === 'folder' ? '/img/folder.svg' : '/img/folder.svg'" alt="folder image">
    <div class="name">{{this.File.name}} ({{new Date(this.File.date).toISOString()}})</div>
  </div>
  <div class="items" v-if="this.File.isExtended && this.File.children.length">
    <current-component v-for="file in this.File.children" :key="file.date" :file="file"/>
  </div>
</template>

<script>
/* eslint-disable */
import CurrentComponent from '@/components/Items.vue'
export default {
  name: 'TheItem',
  props: ["file"],
  data(){
    return {
      name: "name",
      src:"/img/plus.svg",
      File: this.file
    }
  },
  components: {
    CurrentComponent
  },
  methods:{
    toggle(){
      this.File.isExtended = !this.File.isExtended
      // if (this.File.isExtended){
      //   this.$refs.folders
      // }else{
      //
      // }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/*.container{*/
/*  width: 50%;*/
/*  height: 80vh;*/
/*  background-color: #1f1f3a;*/
/*  border-radius: 10px;*/
/*  box-shadow: 0 0 100px black;*/
/*  margin: 80px;*/
/*}*/
h1 {
  color: wheat;
}
.folders{
  display: flex;
  flex-direction: row;
  /*margin-bottom: 10px;*/
  /*margin-top: 10px;*/
  /*margin-right: 10px;*/
  /*margin-left: 43px;*/
  margin: 10px;
}
.images{
  width: 24px;
  height: 24px;
  margin-left: 10px;
  /*background-color: white;*/
}
.name{
  color: white;
  text-align: right;
  margin-left: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
