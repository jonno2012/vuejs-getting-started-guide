<template>
  <slot>Default text</slot>
  <button @click="increment">Click Me!: {{ clicks }}</button>
  <br>
  <button @click="resetClicks">Reset</button>
  <br>
  <ul v-if="todos">
    <li
        v-for="todo in filteredTodos"
        :key="todo.id"
        :class="todo.done ? 'strikeThrough' : ''"
    >{{ todo.text }}<span @click="removeTodo(todo)">  X</span>
      <input type="checkbox" :checked="todo.done" @click="markDone(todo)"></li>
  </ul>
  <input type="text" v-model="text">
  <br>
  <button @click="addTodo" class="whatever" :class="textClass">Add todo</button>
  <div><button @click="hideDoneTodos = !hideDoneTodos">{{ hideDoneTodos ? 'Show' : 'Hide'}} completed todos</button></div>
  <br><br>
  <p>{{ text }}</p>
  <button @click="logoVisible = !logoVisible" :class="classObject">Toggle Logo</button>
  <button @click="externalTodoId++">Fetch external todo {{ externalTodoId+1 }}</button>
  <div v-if="loadExternalTodo">Loading External To do...</div>
  <p v-if="externalTodo">{{ externalTodo }}</p>
  <p><button @click="textActive = !textActive">Toggle Text State</button></p>
</template>

<script>
let id = 0;
export default {
  name: "TodoList",
  data() {
    return {
      text: '',
      textActive: true,
      clicks: 0,
      todos: [
        {id: id++, text: 'Mow lawn', done: false},
        {id: id++, text: 'Do dishes', done: false},
        {id: id++, text: 'Practice code', done: false}
      ],
      loadExternalTodo: false,
      hideDoneTodos: false,
      logoVisible: true,
      externalTodo: null,
      externalTodoId: 0,
      isActive: true
    }
  },
  methods: {
    increment() {
      this.clicks++
    },
    resetClicks() {
      this.clicks = 0
    },
    addTodo() {
      this.todos.push({id: id++, text: this.text, done: false})
      this.text = ''
    },
    removeTodo(todo) {
      this.todos = this.todos.filter((t) => t !== todo)
    },
    markDone(todo) {
      todo.done = !todo.done
      return false
    },
    async fetchExternalTodo() {

      this.externalTodo = null

      this.loadExternalTodo = true

      const res = await fetch(
          `https://jsonplaceholder.typicode.com/todos/${this.externalTodoId}`
      )

      this.loadExternalTodo = false

      this.externalTodo = await res.json()
    }
  },
  computed: {
    textClass() {
      return this.textActive ? 'active' : 'static'
    },
    filteredTodos() {
      return this.hideDoneTodos
          ? this.todos.filter((t) => t.done === false)
          : this.todos
    },
    classObject() {
      return {
        active: this.isActive && !this.error,
        'text-danger': this.error && this.error.type === 'fatal'
      }
    }
  },
  emits: ['showa'],
  created() {
    this.$emit('showa', this.logoVisible)

  },
  watch: {
    logoVisible(e) {
      this.$emit('showa', e)
    },
    externalTodoId() {
      this.fetchExternalTodo()
    }
  }
}
</script>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

p {
  display: block;
}

.static {
  color: red
}

.active {
  color: green
}

.strikeThrough {
  text-decoration: line-through;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>