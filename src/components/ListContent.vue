<template>
    <div class="list-content">
        <ul>
            <li v-for="list in contentLists" :key="list.id">
                <label for="list-checkbox">
                    <input type="checkbox" :checked='list.isChecked'>
                    {{list.name}}
                </label>
                <button class="list-delete" @click="deleteCurList" :value="list.id">删除</button>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        name:'ListContent',
        props:['lists'],
        data() {
            return {
                contentLists:this.lists
            }
        },
        methods: {
            deleteCurList(e){
                // console.log(e.target.value)
                let curId = e.target.value;
                this.contentLists.forEach((curList,index) => {
                    // console.log(curList);
                    // console.log(index);
                    if(curList.id === curId){
                        // console.log(index)
                        this.contentLists.splice(index,1)
                    }
                });

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