<template>
    <div class="list-footer" v-show="allList">
        <input type="checkbox" @change="chooseAll" :checked="isAll">
        已完成<span>{{doneList}}</span> / 全部<span>{{allList}}</span>
        <button class="clear-finish" @click="clearFinished">清除已完成</button>
    </div>
</template>

<script>
    export default {
        name:'ListFooter',
        props:['doneList','allList','isAll','chooseAll','clearFinished'],
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