<script>
import {v4 as uuid} from "uuid";

export default {
  name: "App",
  data() {
    return {
      title: 'Список нотатків',
      text: '',
      errorMessage: '',
      todos: [],
      completeTodo: [],
    };
  },
  methods: {
    addTodo(event) {
      if (event.target.value.trim() === '') {
        this.errorMessage = 'Неможливо створити порожню нотатку'
      } else {
        this.todos.push({id: uuid(), text: event.target.value.trim(), isChanging: false, isComplete: false})
        this.errorMessage = ''
      }
      this.text = ''
    },

    changeTodo(id) {
      this.todos.map(todo => {
        if (todo.id === id) {
          todo.isChanging = !todo.isChanging
        }
      })
    },

    changeTodoText(id, text) {
      console.log(id)
      console.log(text)
      let newObj = {id: id, text: text, isChanging: false, isComplete: false}
      this.todos.forEach((el, idx) => {
        if (el.id === id) {
          this.todos[idx] = newObj
        }
      })
    },

    deleteTodo(id) {
      this.todos.forEach((todo, idx) => {
        if (todo.id === id) {
          this.todos.splice(idx, 1)
        }
        console.log(this.todos)
      })
    },

    completeStatus(initialTodo) {
      console.log(initialTodo)
      this.todos.forEach((todo, idx) => {
        if (todo.id === initialTodo.id) {
          this.todos.splice(idx, 1)
        }
      })
      this.completeTodo.push({...initialTodo, isComplete: true})
      console.log(this.completeTodo)
      console.log(this.todos)
    }
  }
}
</script>

<template>
  <div class="app">
    <header class="header-text"><b>Список нотатків</b></header>
    <input class="input-todo" v-model="text" v-on:keyup.enter="addTodo"/>
    <p style="color:red" class="error-message">{{ errorMessage }}</p>
    <p class="title">Невиконані</p>
    <div class="todo-box">
      <div class="todo" v-for="todo in todos">
        <span v-bind:id="todo.id">

<!--          <input disabled class="form-check-input" type="checkbox" value="" v-bind:id="todo.id">-->
          <label for="{{todo.id}}" @dblclick="changeTodo(todo.id)" v-if="!todo.isChanging"
                 v-bind:id="todo.id">{{ todo.text }}</label>
          <input class="change-todo" v-on:keyup.enter="changeTodoText(todo.id,$event.target.value)" v-else-if="todo.isChanging"
                 v-model="todo.text" v-bind:id="todo.id">
          <div>
            <button @click="completeStatus(todo)" type="button" class="btn btn-success"
                  v-bind:id="todo.id">Виконано</button>
          <button @click="deleteTodo(todo.id)" type="button" class="btn btn-danger"
                    v-bind:id="todo.id">Видалити</button>
          </div>
        </span>
      </div>
    </div>
    <p class="title">Виконані</p>
    <div class="todo-box">
      <div class="todo" v-for="todo in completeTodo">
        <span v-bind:id="todo.id">
<!--          <input class="form-check-input" type="checkbox" value="" v-bind:id="todo.id" disabled checked>-->
          <label for="{{todo.id}}" @dblclick="changeTodo(todo.id)" v-if="!todo.isChanging"
                 v-bind:id="todo.id">{{ todo.text }}</label>
          <input v-on:keyup.enter="changeTodoText(todo.id,$event.target.value)" v-else-if="todo.isChanging"
                 v-model="todo.text" v-bind:id="todo.id">
        </span>
      </div>
    </div>
    <div class="count">Кількість виконаних завдань: <b>{{ completeTodo.length }}</b></div>
  </div>
</template>

<style scoped>

</style>
