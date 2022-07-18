<template>
  <div id="app">
    <div class="todo-list">
      <h3>待办事项</h3>
      <list-header :lists="lists"></list-header>
      <list-content :lists="lists"></list-content>
      <list-footer :lists="lists"></list-footer>
    </div>
  </div>
</template>

<script>
import ListHeader from './components/ListHeader.vue'
import ListContent from './components/ListContent.vue'
import ListFooter from './components/ListFooter.vue'

export default {
  name: 'App',
  components: {
    ListHeader,
    ListContent,
    ListFooter
  },
  data() {
    return {
      lists:[
          {id:'001',name:'工作',isChecked:1},
          {id:'002',name:'学习',isChecked:0},
          {id:'003',name:'娱乐',isChecked:1}
      ]
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body,h1,h2,h3,h4,h5,ul,input{
  margin: 0;
  padding: 0;
  font-size: 14px;
}
ul li{
  list-style: none;
}

.todo-list{
  width: 600px;
  border: 1px solid #999;
  padding: 8px;
  padding-bottom: 0;
  margin: 0 auto;
  border-radius: 8px;
}

.todo-list input{
  font-size: 14px;
  padding: 9px;
  border:1px solid #999;
  border-radius: 3px;
  box-sizing: border-box;
}
</style>
