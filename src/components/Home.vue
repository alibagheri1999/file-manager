<template>
  <div class="container">
    <h1>File Manager</h1>
    <Items v-for="file in files" :key="file.date" :file="file"/>
  </div>
</template>

<script>
/* eslint-disable */

import Items from "@/components/Items";

export default {
  name: 'TheHome',
  props: {
  },
  components: {
    Items
  },
  data(){
    return {
      stat: false,
      files: [
        {
          name: "first folder",
          type: 'folder',
          date: Date.now(),
          children: [{
            name: "ali's folder",
            type: 'folder',
            date: Date.now(),
            children: [{
              name: "reza's folder",
              type: 'folder',
              date: Date.now(),
              children: [],
              isExtended: false,
            },
              {
                name: "bagheri's folder",
                type: 'folder',
                date: Date.now() + 10000,
                children: [{
                  name: "mohammad3's folder",
                  type: 'folder',
                  date: Date.now() + 10000,
                  children: [],
                  isExtended: false,
                }],
                isExtended: false,
              },],
            isExtended: false,
          },
            {
              name: "mohammad's folder",
              type: 'folder',
              date: Date.now() + 10000,
              children: [{
                name: "mohammad2's folder",
                type: 'folder',
                date: Date.now() + 10000,
                children: [],
                isExtended: false,
              }],
              isExtended: false,
            },
            ],
          isExtended: false,
        },
        {
          name: "second folder",
          type: 'folder',
          date: Date.now() + 10000,
          children: [],
          isExtended: false,
        },
        {
          name: "third folder",
          type: 'folder',
          date: Date.now() + 20000,
          children: [],
          isExtended: false,
        }
      ],
      name: "name",
      src:"/img/plus.svg"
    }
  },
  methods:{
    toggle(){
      if (this.src === "/img/minus.svg"){
        this.src = "/img/plus.svg"
      }else{
        this.src = "/img/minus.svg"
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container{
  width: 50%;
  height: 80vh;
  background-color: #1f1f3a;
  border-radius: 10px;
  box-shadow: 0 0 100px black;
  margin: 80px;
  overflow-x: auto;

}
h1 {
  color: wheat;
}
</style>
