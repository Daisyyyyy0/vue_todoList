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
    <main class="main" v-show="todos.length">
      <input 
        id="toggle-all" 
        type="checkbox" 
        class="toggle-all" 
      />
      <label for="toggle-all"></label>
      <ul class="todo-list">
        <li class="todo" 
        :class="{completed: todo.completed}"
        v-for="todo in filteredTodos"
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
    <footer class="footer"  v-show="todos.length">
      <span class="todo-count"> 
        <strong>{{ remaining }}</strong> {{remaining | pluralize}} left 
      </span>
      <ul class="filters">
        <li>
          <a href="#/all" 
            @click.stop.prevent="setVisibility('all')"
            :class="{ selected: visibility === 'all'}"
            >All</a>
        </li>
        <li>
          <a href="#/active" 
            @click.stop.prevent="setVisibility('active')"
            :class="{ selected: visibility === 'active'}"
            >Active</a>
        </li>
        <li>
          <a href="#/completed" 
            @click.stop.prevent="setVisibility('completed')"
            :class="{ selected: visibility === 'completed'}"
            >Completed</a>
        </li>
      </ul>
      <button class="clear-completed" @click="removeCompleted">
        Clear completed
    </button>
    </footer>
      <button class="btnn" type="button" @click.stop.prevent="saveStorage">
        Save
      </button>
  </section>

</template>

<script>
// import "todomvc-app-css/index.css"
import { v4 as uuidv4 } from 'uuid';

const STORAGE_KEY = 'todomvc-app-vue'
const filters = {
  all: (todos) => todos,
  active: (todos) => todos.filter((todo) => !todo.completed),
  completed: (todos) => todos.filter((todo) => todo.completed),
}


export default {
  name: "todo",
  data() {
    return {
        visibility: 'all',
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
        currentEditTodo: {

        },

    }
  },
  methods:{
    addTodo(){
        const title = this.newTodo && this.newTodo.trim()
        if(!title) { return } 
        console.log("this.newTodo ", this.newTodo);

        this.todos.push({
            id: uuidv4(),
            title: this.newTodo,
            completed: false
        })
        this.newTodo= ''
    },
    removeTodo(todo){
        this.todos = this.todos.filter(_todo => _todo.id !==todo.id)
    },
    setVisibility(visibility){
        // console.log('user choose visibility is:', visibility)
        this.visibility = visibility
    },
    removeCompleted(){
        this.todos = filters.active(this.todos)
    },
    saveStorage() {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todos))
    }

  },
  computed:{
    remaining(){
        // console.log('active: ', filters.active(this.todos));
        return filters.active(this.todos).length
    },
    filteredTodos(){
        return filters[this.visibility](this.todos)
    },
  },
  filters:{
    pluralize(n) {
      return n === 1 ? "item" : "items";
    }
  },
  watch:{
    todos:{
        handler:function(){
            console.log('saveStorage');
            this.saveStorage()
        },
        deep: true,
    }
  },
  created(){
    this.todos = JSON.parse(localStorage.getItem(STORAGE_KEY)) || []
  },


};
</script>

<style>
    .todoapp{
        margin-left: 50px;
    }
    .footer{
        height:40px;
    }

    .btnn{
        z-index: 22;
        position: relative;
    }
</style>
