<template>
    <div class="list-header">
        <input type="text" placeholder="请输入你的任务，按回车键确认" @keydown.enter="getValue">
    </div>
</template>

<script>
    export default {
        name:'ListHeader',
        props:['lists'],
        data() {
            return {
                headerLists:this.lists
            }
        },
        methods: {
            getValue(e){
                // console.log(e.target.value)
                // console.log(this.headerLists)
                let curValue = e.target.value.trim();
                let randomNum = Math.floor(Math.random() * 10000).toString();
                if(curValue === '' || curValue === null || curValue === undefined){
                    alert('您输入的结果为空，请重新输入。')
                    return false;
                }
                this.headerLists.push({'id':randomNum,name:curValue,isChecked:0})
            }
        },
    }
</script>

<style scoped>
    .list-header{
        margin-top: 20px;
    }
    .list-header input{
        width: 100%;
        height: 40px;
    }
</style>