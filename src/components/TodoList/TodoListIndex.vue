<script setup lang="ts">
import { computed, type ModelRef } from 'vue'
import type { Todo } from '../models'
import TodoCompleted from './TodoCompleted.vue'
import TodoInprogress from './TodoInprogress.vue'

const todos = defineModel('todos') as ModelRef<Todo[]>
const props = defineProps(['selectedCategory'])
const removeTodo = (todo: Todo) => {
  todos.value = todos.value.filter((t) => t !== todo)
}
const filteredTodos = computed(() => {
  return todos.value.filter((todo) => props.selectedCategory === todo.category);
})
</script>
<template>
  <TodoInprogress v-model:todos="filteredTodos" :removeTodo="removeTodo" />
  <TodoCompleted v-model:todos="filteredTodos" :removeTodo="removeTodo" />
</template>

<style>
.task-btn:hover {
  cursor: pointer;

  h3 {
    font-weight: bold;
  }
}
</style>
