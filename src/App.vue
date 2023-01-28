<template>
  <div class="holder">
    <Todo />
  </div>
</template>

<script>
import Todo from "./components/Todo.vue"

export default {
  name: 'App',
  components: {
    Todo
  }
}
</script>

<style scoped lang="css">
  @import "./assets/global.css"
</style>