<script setup lang="ts">
import { computed, type ModelRef } from 'vue';
import type { Todo } from '../models';
import TodoCompleted from './TodoCompleted.vue';
import TodoInprogress from './TodoInprogress.vue';

const todos = defineModel('todos') as ModelRef<Todo[]>;
const props = defineProps({
  filteredCategory: Array,
});
const removeTodo = (todo: Todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};
const filteredTodos = computed(() => {
  return todos.value.filter((todo) => props.filteredCategory?.includes(todo.category));
});
</script>
<template>
  <section class="q-pa-md">
    <TodoInprogress
      class="q-mb-lg"
      v-model:todos="filteredTodos"
      :removeTodo="removeTodo"
    />
    <TodoCompleted
      class="q-mb-lg"
      v-model:todos="filteredTodos"
      :removeTodo="removeTodo"
    />
  </section>
</template>

<style lang="scss">
.done {
  .todo-content {
    text-decoration: line-through;
    color: #bbb;
  }
}
</style>
