<template>
  <div id="app">
    <div class="todo-list">
      <h3>待办事项</h3>
      <list-header @addTodo="addTodo"></list-header>
      <list-content :lists="lists" @editTodo="editTodo"></list-content>
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
import pubsub from 'pubsub-js'
import ListHeader from './components/ListHeader.vue'
import ListContent from './components/ListContent.vue'
import ListFooter from './components/ListFooter.vue'

export default {
  name: 'App',
  components: {ListHeader,ListContent,ListFooter},
  data() {
    return {
      lists: JSON.parse(localStorage.getItem('todos')) || []//读取localStorage里的todos字符串，转换为数组对象，初始化时为空数组
    }
  },
  methods:{
    //用一个函数里的参数接收ListHeader子组件传过来的todo对象，添加到lists数组中
    addTodo(todo){
      //console.log('我是app父组件，我收到了子组件传过来的todo对象：',todo)
      this.lists.unshift(todo);
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
    editTodo(curdId,curTitle){
      this.lists.forEach(todo => {
        if(todo.id == curdId){
          todo.title = curTitle;
        }
      });
    },
    //用一个函数里的参数接收ListContent子组件传递过来的curId字符串，修改当前项的done值
    checkedChange(name,curdId){
      // console.log(cudId)
      this.lists.forEach(curList => {
          if(curList.id === curdId){
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
    },
  },
  watch:{
    //监测lists数组，添加到localStorage
    lists:{
      deep:true,//开启深度监视，能够监测到数组内层数据的变化。不开启只能监测到数组这一层
      handler(value){
        localStorage.setItem('todos',JSON.stringify(value))
      }
    },
  },
  mounted(){
    this.$bus.$on('deleteCurList',this.deleteCurList)//全局事件总线方法接收数据
    this.pubId = pubsub.subscribe('checkedChange',this.checkedChange)//用订阅消息方法接收数据
  },
  beforeDestroy(){
    this.$bus.$off('deleteCurList')//解绑$bus上的事件
    pubsub.unsubscribe(pubId)//取消订阅
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

.global-btn{
    padding: 5px 8px;
    margin-top: -2px;
    background: #42b983;
    color: #fff;
    border: none;
    border-radius: 3px;
    cursor: pointer;
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
