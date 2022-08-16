<template>
  <section class="todoapp">
    <header>
      <h1>todos</h1>
    </header>
    <input 
      type="text" 
      class="new-todo" 
      autofocus 
      placeholder="需要做什麼？" 
      v-model="newTodo"
      @keyup.enter="addTodo"
    />
    <main class="main">
      <input 
        id="toggle-all" 
        type="checkbox" 
        class="toggle-all" 
      />
      <label for="toggle-all"></label>
      <ul class="todo-list">
        <li class="todo" 
        :class="{completed: todo.completed}"
        v-for="todo in todos"
        :key="todo.id"
        >
          <div class="view">
            <input v-model="todo.completed" type="checkbox" class="toggle" />
            <label for="">{{ todo.title }}</label>
            <button class="destroy" @click="removeTodo(todo)"></button>
          </div>
          <input type="text" class="edit" />
        </li>

      </ul>
    </main>
    <footer class="footer">
      <span class="todo-count"> <strong>3</strong> items left </span>
      <ul class="filters">
        <li>
          <a href="#/all">All</a>
        </li>
        <li>
          <a href="#/active">Active</a>
        </li>
        <li>
          <a href="#/completed">Completed</a>
        </li>
      </ul>
      <button class="clear-completed">Clear completed</button>
    </footer>
  </section>
</template>

<script>
// import "todomvc-app-css/index.css"
import { v4 as uuidv4 } from 'uuid';

export default {
  name: "todo",
  data() {
    return {
        // isCompleted: true,
        newTodo: '',
        todos:[
            {
                id: uuidv4(),
                title: '洗衣服',
                completed: true,
            },
            {
                id: uuidv4(),
                title: '棒式運動',
                completed: false,
            },
            {
                id: uuidv4(),
                title: '吃晚餐',
                completed: false,
            },
        ],

    }
  },
  methods:{
    addTodo(){
        const title = this.newTodo && this.newTodo.trim()
        if(!title) { return } 
        console.log("this.newTodo: ", this.newTodo);

        this.todos.push({
            id: uuidv4(),
            title: this.newTodo,
            completed: false
        })
        this.newTodo= ''
    },
    // removeTodo(todo){
    //     console.log('remove todo', todo.id)
    //     this.todos = this.todos.filter(_todo => _todo.id !==todo.id)
    // },
    removeTodo: () => {
        console.log('this', this);
    }
  }
};
</script>

<style>
    .todoapp{
        margin-left: 50px;
    }
    .footer{
        height:40px;
    }
</style>
