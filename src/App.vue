<template>
  <div id="app">
    <h1>X TODO</h1>
    <todo-list></todo-list>
  </div>
</template>

<script>
import TodoList from './components/todo-list'

export default {
  name: 'app',
  data () {
    return {
      name: 'App'
    }
  },
  components: {
    TodoList
  }
}
</script>

<style>
#app {
  text-align: center;
  margin-top: 60px;
}
</style>
