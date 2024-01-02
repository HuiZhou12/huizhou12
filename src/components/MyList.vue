<template>
    <ul class="todo-main">
        <transition-group>
        <MyItem v-for="todoObj in todos" :key="todoObj.id" :todo="todoObj"/>
        </transition-group>
    </ul>
</template>

<script>
import MyItem from './MyItem.vue'

export default {
    name:"MyList",
    components:{
        MyItem,
    },
    //声明tods的值并接收
    props:['todos']

}
</script>

<style>
.todo-main{
margin-left: 0px;
border: 1px solid #ddd;
border-radius: 2px;
padding: 0px;
}
.todo-empty{
height: 40px;
line-height: 40px;
border: 1px solid #ddd;
border-radius: 2px;
padding-left: 5px;
margin-top: 10px;
}
.v-leave-to{
  transform: translateX(-100%);
}
.v-leave-active{
  transition: 0.5s linear;
}
.v-leave{
  transform: translateX(0);
}
</style>