<template>
    <div class="list-footer">
        <input type="checkbox" @click="chooseAll" :checked="isAllChecked">
        已完成<span>{{finishedList}}</span> / 全部<span>{{lists.length}}</span>
        <button class="clear-finish">清除已完成</button>
    </div>
</template>

<script>
    export default {
        name:'ListFooter',
        props:['lists'],
        data() {
            return {
                isAllChecked:0
            }
        },
        computed:{
            finishedList(){
                let i = 0;
                this.lists.forEach(list => {
                    if(list.isChecked){
                        i++;
                    }
                });
                return i;
                // return this.lists.reduce((prev,current)=> prev + (current.isChecked === 1 ? 1 : 0),0)//reduce写法，更简洁
            }
        },
        methods:{
            chooseAll(){

            }
        },
    }
</script>

<style scoped>
    .list-footer{
        text-align: left;
        padding: 20px 0 20px 9px;
    }
    .list-footer input{
        vertical-align: middle;
        margin-top: -3px;
    }
    .list-footer .clear-finish{
        float: right;
        padding: 8px;
        margin-top: -6px;
        background: #42b983;
        color: #fff;
        border: none;
        border-radius: 3px;
        cursor: pointer;
    }
</style>