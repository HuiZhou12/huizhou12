<template>
    <div class="todo-footer" v-show="total()">
        <label>
            <input type="checkbox" :checked="isAll()" @change="checke"/>
        </label>
        <span>
            <span>已完成{{tododone()}}</span>/全部{{total()}}
        </span>
        <button class="btn btn-danger" @click="ClearAll">清理已完成的任务</button>
    </div>
</template>

<script>
export default {
    name:"MyFooter",
    props:['todos'],
    methods:{
      total(){
        return this.todos.length 
      },
      tododone(){
      return this.todos.reduce((pre,current)=>{
          return pre + (current.done? 1 :0)
        },0)
      },
      isAll(){
        return this.tododone() === this.total() && this.total() > 0
      },
      checke(e){
        this.$emit('selectAll',e.target.checked)
      },
      ClearAll(){
        this.$emit('clearAll')
      }
    }
}
</script>

<style>
.todo-footer{
  height: 10px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}
.todo-footer lable{
  display:  inline-block;
  margin-right: 20px;
  cursor: pointer;
}
.todo-footer lable input{
  position: relative;
  top: 1px;
  vertical-align: middle;
  margin-right: 5px;
}
.todo-footer button{
  float: right;
  margin-top: 5px;
}
</style>