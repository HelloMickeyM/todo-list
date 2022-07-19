<template>
  <div id="app">
    <div class="todo-list">
      <h3>待办事项</h3>
      <list-header :addTodo="addTodo"></list-header>
      <list-content
       :lists="lists"
       :deleteCurList="deleteCurList"
       :checkedChange="checkedChange">
      </list-content>
      <list-footer
       :doneList="doneList"
       :allList="allList"
       :isAll="isAll"
       :chooseAll="chooseAll"
       :clearFinished="clearFinished">
      </list-footer>
    </div>
  </div>
</template>

<script>
import ListHeader from './components/ListHeader.vue'
import ListContent from './components/ListContent.vue'
import ListFooter from './components/ListFooter.vue'

export default {
  name: 'App',
  components: {ListHeader,ListContent,ListFooter},
  data() {
    return {
      lists:[
          {id:'001',title:'工作',done:true},
          {id:'002',title:'学习',done:false},
          {id:'003',title:'娱乐',done:true}
      ]
    }
  },
  methods:{
    //用一个函数里的参数接收ListHeader子组件传过来的todo对象，添加到lists数组中
    addTodo(todo){
      //console.log('我是app父组件，我收到了子组件传过来的todo对象：',todo)
      this.lists.unshift(todo)
    },
    //用一个函数里的参数接收ListContent子组件传递过来的curId字符串，删除当前项
    deleteCurList(curId){
      // console.log(e.target.value)
      this.lists.forEach((curList,index) => {
          // console.log(curList);
          // console.log(index);
          if(curList.id === curId){
              // console.log(index)
              if(confirm('确定删除吗？')){
                  this.lists.splice(index,1)
              }
          }
      });
    },
    //用一个函数里的参数接收ListContent子组件传递过来的curId字符串，修改当前项的done值
    checkedChange(cudId){
      // console.log(cudId)
      this.lists.forEach(curList => {
          if(curList.id === cudId){
              curList.done = !curList.done;
          }
      })
    },
    //全选或取消全选
    chooseAll(e){
        //console.log(e.target.checked)
        this.lists.forEach(list => {
            list.done = e.target.checked
        });
    },
    //清空已完成
    clearFinished(){
      if(confirm('确定清空已完成吗？')){
        this.lists = this.lists.filter(list => {
            return !list.done;
        })
      }
    }
  },
  computed:{
    //统计已完成个数
    doneList(){
        let i = 0;
        this.lists.forEach(list => {
            if(list.done){
                i++;
            }
        });
        return i;
        // return this.lists.reduce((prev,current)=> prev + (current.isChecked ? 1 : 0),0)//reduce写法，更简洁
    },
    //todo总个数
    allList(){
        return this.lists.length
    },
    //判断是否全选
    isAll(){
        return this.doneList == this.allList && this.allList > 0;
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
