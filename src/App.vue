<template>
  <v-app>
    <v-main class="bg-pink-lighten-4">
      <v-container>
        <h1 class="text-center">To-Do List</h1>
        <TodoList />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import TodoList from './components/TodoList.vue';

export default {
  components: {
    TodoList
  }
};
</script>

<style>
</style>
