<template>
    <div class="list-header">
        <input type="text" placeholder="请输入你的任务，按回车键确认" @keyup.enter="add">
    </div>
</template>

<script>
import {nanoid} from 'nanoid'

    export default {
        name:'ListHeader',
        methods: {
            add(e){
                //通过事件对象获取输入的值，清除前后空格，如果获取表单里多个值还可以用v-model
                let curValue = e.target.value.trim();
                //判断是否为空
                if(curValue === '' || curValue === null || curValue === undefined){
                    alert('您输入的结果为空，请重新输入。')
                    return false;
                }
                //包装好要添加的内容
                let todo = {id:nanoid(),title:curValue,done:false};
                //通过自定义事件将包装好的数据传递给父组件
                this.$emit('addTodo',todo)
                //添加完之后清空输入框
                e.target.value = ''
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