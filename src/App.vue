<template>
  <div class="container">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">Simple todo App</h5>
        <div class="row">
          <div class="col-10">
            <input type="text" class="form-control" v-model="todo" @keyup.enter="add">
          </div>
          <div class="col-2">
            <button class="btn btn-success justify-content-end" @click="add">Add</button>
          </div>
        </div>
        <List :todos="todos" @destroy="deleteTodo" @done="doneTodo" />
        <small>total todo: {{totalTodo}}</small> || <small>done todo: {{todoDone}}</small>
      </div>
    </div>
  </div>
</template>

<script>
import List from './components/List.vue';

export default {
  components: {List},
  data() {
    return {
      todo: '',
      todos: []
    }
  },
  mounted() {
    this.todos = JSON.parse(localStorage.getItem('todos'))
  },
  computed: {
    totalTodo() {
      return this.todos.length
    },
    todoDone() {
      return this.todos.filter(item => item.status).length
    }
  },
  methods: {
    add() {
      this.todos.unshift({
        status: false,
        activity: this.todo
      })
      this.todo=""
      this.saveToLocalStorage()
    },
    deleteTodo(todoIndex) {
      this.todos = this.todos.filter((item, index) => {
        if(index != todoIndex) {
          return item
        }
      })
      this.saveToLocalStorage()
    },
    doneTodo(todoIndex) {
      this.todos = this.todos.filter((item, index) => {
        if(index == todoIndex) {
          item.status = !item.status
        }
        return item
      })
      this.saveToLocalStorage()
    },
    saveToLocalStorage() {
      localStorage.setItem('todos', JSON.stringify(this.todos))
    }
  }
}
</script>
