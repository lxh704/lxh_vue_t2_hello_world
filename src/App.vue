<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <TodoHeader
            @addNewTodo="addTodo"/>
        <TodoList
            :todos="todos"
            :checkTodo="checkTodo"
            :deleteTodo="deleteTodo"
        />
        <TodoFooter
            :todos="todos"
            @checkAllTodo="checkAllTodo"
            @clearAllDoneTodo="clearAllDoneTodo"
        />
      </div>
    </div>
  </div>
</template>

<script>
import TodoHeader from "@/components/TodoList/TodoHeader";
import TodoList from "@/components/TodoList/TodoList";
import TodoFooter from "@/components/TodoList/TodoFooter";

export default {
  name: "App",
  components: {
    TodoList,
    TodoFooter,
    TodoHeader
  },
  watch: {
    todos: {
      deep: true,
      handler(newValue, oldValue) {
        localStorage.setItem("todos", JSON.stringify(newValue));
      }
    }
  },
  data() {
    return {
      // todos: [
      //   {id: '001', title: '吃饭', done: false},
      //   {id: '002', title: "睡觉", done: true},
      //   {id: '003', title: '打代码', done: false}
      // ]
      todos: JSON.parse(localStorage.getItem("todos")) || [],
    }
  },
  methods: {
    //添加的todo
    addTodo(todo) {
      console.log('我是app组件，我收到了数据');
      this.todos.unshift(todo);
    },
    checkTodo(id) {
      const todo = this.todos.find(todo => todo.id === id);
      todo.done = !todo.done;
    },
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    checkAllTodo(done) {
      this.todos.forEach(todo => todo.done = done);
    },
    clearAllDoneTodo() {
      this.todos = this.todos.filter(todo => !todo.done)
    }
  }
  ,mounted() {
   // console.log('我是app组件，我挂载了',this.$bus);
    this.$bus.$on('checkTodo', this.checkTodo);
    this.$bus.$on('deleteTodo', this.deleteTodo);

    this.$bus.$on('editTodo', (id,title)=>{
      const todo = this.todos.find(todo => todo.id === id);
      todo.title = title
    });
  }
  ,
  beforeDestroy() {
    this.$bus.$off('checkTodo', this.checkTodo);
    this.$bus.$off('deleteTodo', this.deleteTodo);
    this.$bus.$off('editTodo', this.addTodo);

  }
}
</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}
.btn-Edit {
  color: #fff;
  background-color: #4981da;
  border: 1px solid #bd362f;
  margin-right: 5px;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}



.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}

.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

</style>


