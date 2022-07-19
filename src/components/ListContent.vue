<template>
    <div class="list-content">
        <ul>
            <li v-for="list in lists" :key="list.id">
                <label for="list-checkbox">
                    <input type="checkbox" :checked='list.done' @click="checkedThis(list.id)">
                    {{list.title}}
                </label>
                <button class="list-delete" @click="deleteThis" :value="list.id">删除</button>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        name:'ListContent',
        props:['lists','deleteCurList','checkedChange'],
        methods: {
            //将当前todo项的id传递给父组件
            deleteThis(e){
                let curId = e.target.value;
                this.deleteCurList(curId)
            },
            //将当前todo项的id传递给父组件
            checkedThis(cudId){
                // console.log(cudId)
                this.checkedChange(cudId)
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
    }
    .list-content input{
        vertical-align: middle;
        margin-top: -3px;
    }
    .list-content .list-delete{
        display: none;
        float: right;
        padding: 3px 8px;
        margin-top: -2px;
        background: #42b983;
        color: #fff;
        border: none;
        border-radius: 3px;
        cursor: pointer;
    }
    .list-content li:hover .list-delete{
        display: block;
    }
</style>