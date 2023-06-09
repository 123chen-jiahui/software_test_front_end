<template>
    <div class="sidebar">
      <div>
        <ul id="txt" :class="{ 'active': activeIndex === index }" @click="handleItemClick(index,item)" v-for=" ( item,index) in items" :key="index">{{ item }}</ul>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios'
  //import eventBus from 'src/App.vue'
//   import Vue from 'vue';
//   const eventBus = new Vue();
  export default {
    data() {
      return {
        items_string: "",
        items: [],
        activeIndex: -1
      }
      },
      methods: {
          handleItemClick(index,item) { //左边栏选择
              this.activeIndex = index
              let selected_name = item  
              console.log("==selected_name==")
              console.log(selected_name)
              this.$parent.eventBus.$emit("testname",selected_name) //传输选择的测试文件名到HeadPart
          },

      },
    mounted() {
      // 发送GET请求获取数据
      axios.get('http://localhost:8888/readAll')
        .then(response => {
          this.items_string = response.data
          this.items = this.items_string.split(" ")
          console.log(this.items)
        })
        .catch(error => {
          console.log(error)
        })
    }
  }
  </script>
  
  <style>
  .sidebar {
    width: 25%;
    height: 100%;
    background-color: #8aab92;
    position:fixed;
    top:62px;
    min-height: 100vh; /* 设置最小高度为100vh，即视口高度 */
  }
  .sidebar #txt{
    color: #fff;
    font-weight: 800;
    margin-left:0px;
    padding-left:0px;
    cursor: pointer;
  }
  .active {
  background-color: rgb(58, 82, 69);
}
  </style>