<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @addTodo="addTodo"/>
        <!-- 它们用于在父组件中引入和使用子组件，并通过props将数据传递给子组件 -->
        <MyList :todos="todos"/>
        <MyFooter :todos="todos" @selectAll="selectAll" @clearAll="clearAll"/>
      </div>
    </div>
  </div>
</template>

<script>
import MyHeader from'./components/MyHeader.vue'
import MyFooter from'./components/MyFooter.vue'
import MyList from'./components/MyList.vue'

export default {
  name:'App',
  components:{
    MyFooter,
    MyHeader,
    MyList,
  },
  data(){
      return{
          todos:JSON.parse(localStorage.getItem('todos')) || []
      }
    },
    methods:{
      //添加数据的方法
      addTodo(todoObj){
        this.todos.unshift(todoObj)
      },
      //记录数据的完成状态方法
      checkTodo(id){
        this.todos.forEach(function(todos){
          if(todos.id === id){
            todos.done = !todos.done
          }
        })
      },
      //删除数据方法
      deleteTodo(x) {
        this.todos = this.todos.filter(function(todos) {
          return todos.id !== x;
        });
      },
      //更新数据方法
      updateTodo(todoId,data){
        this.todos.forEach(function(todos){
          if(todoId == todos.id){
            todos.title = data
          }
        })
      },
      //全选数据方法
      selectAll(a){
        this.todos.forEach((todos)=>{
          todos.done = a
        })
      },
      //清除已完成的事情方法
      clearAll(){
        this.todos = this.todos.filter((todos)=>{
          return !todos.done
        })
      }      
    },
    mounted(){
      this.$bus.$on('checkTodo',this.checkTodo)
      this.$bus.$on('deleteTodo',this.deleteTodo)
      this.$bus.$on('updateTodo',this.updateTodo)
    },
    beforeDestroy(){
      this.$off(['checkTodo','deleteTodo','updateTodo'])
    },
    watch:{
      todos:{
        deep:true,
        handler(value){
          localStorage.setItem('todos',JSON.stringify(value))
        }
      }
    },
}
</script>

<style>
body{
  background: #fff;
}
.btn{
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
}
.btn-danger{
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}
.btn-edit{
  color: #fff;
  background-color: skyblue;
  border: 1px solid rgb(97, 167, 194);
  margin-right: 5px;
}
.btn-edit:hover{
  color: #fff;
  background-color: rgb(101, 154, 175);
  border: 1px solid rgb(64, 108, 126);
  margin-right: 5px;
}
.btn-danger:hover{
  color:#fff;
  background-color: #bd362f;
}
.btn:focus{
  outline: none;
}
.todo-container{
  width: 600px;
  margin: 0 auto;
}
.todo-container rodo-wrap{
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

</style>
