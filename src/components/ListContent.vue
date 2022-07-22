<template>
    <div class="list-content">
        <ul>
            <li v-for="list in lists" :key="list.id">
                <label for="list-checkbox">
                    <input type="checkbox"
                     :checked='list.done'
                     @click="checkedThis(list.id)"
                    >
                    <span class="list-title" v-show="!list.isEdit">{{list.title}}</span>
                    <input
                     type="text"
                     class="edit-input"
                     v-show="list.isEdit"
                     v-bind:value="list.title"
                     @blur="blurThis(list,$event)"
                     ref="inputTitle"
                    >
                </label>
                <button class="global-btn list-edit" @click="editThis(list)" v-show="!list.isEdit">编辑</button>
                <button class="global-btn list-delete" @click="deleteThis(list.id)">删除</button>
            </li>
        </ul>
    </div>
</template>

<script>
    import pubsub from 'pubsub-js'
    export default {
        name:'ListContent',
        props:['lists'],
        methods: {
            //将当前todo项的id传递给父组件
            deleteThis(curdId){
                // this.deleteCurList(curId)
                this.$bus.$emit('deleteCurList',curdId)//全局事件总线方法发送数据
            },
            //将当前todo项的id传递给父组件
            checkedThis(curdId){
                // console.log(cudId)
                pubsub.publish('checkedChange',curdId)//用订阅消息方法发送数据
            },
            editThis(curList){
                // console.log(curList)
                if(curList.hasOwnProperty('isEdit')){
                    curList.isEdit = true
                }else{
                    this.$set(curList,'isEdit',true)
                };
                this.$nextTick(function(){
                    // this.$refs.inputTitle[0].focus();
                    this.$refs.inputTitle.forEach((item)=>{
                        item.focus();
                    })
                })
            },
            blurThis(curList,e){
                // console.log('失去焦点了')
                curList.isEdit = false;
                let curTitle = e.target.value;
                this.$emit('editTodo',curList.id,curTitle)
            }
        },
    }
</script>

<style scoped>
    .list-content{
        margin-top: 20px;
    }
     .list-content ul{
        border-radius: 3px;
        border:1px solid #999;
        border-bottom: 0;
    }
    .list-content li{
        text-align: left;
        padding: 9px;
        border-bottom: 1px solid #999;
        box-sizing: border-box;
        display: flex;
        align-items: center;
    }
    .list-content li label{
        display: flex;
        align-items: center;
        flex: 1;
    }
    .list-content input{
        vertical-align: middle;
    }
    .list-content .list-title{
        display: block;
        padding: 5px 0;
        margin-left: 5px;
    }
    .list-content .list-delete{
        display: none;
    }
    .list-content .list-edit{
        display: none;
        margin-right: 5px;
    }
    .list-content li:hover .list-delete{
        display: block;
    }
    .list-content li:hover .list-edit{
        display: block;
    }
    .list-content .edit-input{
        padding: 4px;
    }
</style>