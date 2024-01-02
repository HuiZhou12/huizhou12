<template>
    <li>
        <label>
        <input type="checkbox" :checked="todo.done" @change="checkTodo(todo.id)"/>
        <span v-show="!todo.isEdit">{{todo.title}}</span>
        <input type="text" v-show="todo.isEdit" :value="todo.title"  @keyup.enter="editDone(todo,$event)" ref="furce" >
        </label>
        <button class="btn btn-danger" v-show="!todo.isEdit"  @click="del(todo.id)">删除</button>
        <button class="btn btn-edit" v-show="!todo.isEdit"  @click="edit(todo)" >编辑</button>
    </li>
</template>

<script>
export default {
    name:'MyItem',
    //声明接收todo值
    props:['todo'],
    mounted(){
        // console.log(this);
    },
    methods:{
      checkTodo(id){
        this.$bus.$emit('checkTodo',id)
      },
      del(x){
          alert('确定要删除吗')
          // this.deleteTodo(x)
          this.$bus.$emit('deleteTodo',x)
      },
      //编辑
      edit(todo){
        if(hasOwnProperty.call(todo, 'isEdit')){
          todo.isEdit = true
        }else{
          //在一个对象里面添加了一个元素，并且添加get和set调用
          this.$set(todo,'isEdit',true)
        }
        this.$nextTick(function(){
          this.$refs.furce.focus()
        })
      },
      //编辑完成，按回车触发该事件
      editDone(todo,e){
        todo.isEdit=false
        this.$bus.$emit('updateTodo',todo.id,e.target.value)
      }
    }
}
</script>

<style scoped>
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background-color: #ddd;
}

li:hover button {
  display: block;
}
</style>