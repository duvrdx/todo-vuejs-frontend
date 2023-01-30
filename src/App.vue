<template>
  <div class="holder">
    <div class="imageHolder">
      <img src="./assets/title.png" alt="Minha Figura">
    </div>
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