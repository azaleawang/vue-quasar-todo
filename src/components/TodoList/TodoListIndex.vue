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

<script setup lang="ts">
import { computed, type ModelRef, inject, type Ref } from 'vue';
import type { Todo } from '../models';
import TodoCompleted from './TodoCompleted.vue';
import TodoInprogress from './TodoInprogress.vue';

const todos = defineModel('todos') as ModelRef<Todo[]>;
const filteredCategory = inject('filteredCategory') as Ref<string[]>;
const removeTodo = (todo: Todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};

const filteredTodos = computed(() => {
  return todos.value.filter((todo) =>
    filteredCategory.value.includes(todo.category)
  );
});
</script>

<style lang="scss">
.done {
  .todo-content {
    text-decoration: line-through;
    color: #bbb;
  }
}
</style>
