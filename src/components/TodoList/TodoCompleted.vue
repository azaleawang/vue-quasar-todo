<script setup lang="ts">
import { ref, computed, type ModelRef, defineProps } from 'vue';
import type { Todo } from '../models';

const todos = defineModel('todos') as ModelRef<Todo[]>;

const showCompleted = ref(false);

const todosCompleted = computed(() => {
  return todos.value
    .filter((todo) => todo.done)
    .sort((a, b) => b.createdAt - a.createdAt);
});

defineProps({
  removeTodo: {
    type: Function,
    default: () => () => console.warn('removeTodo not provided'),
  },
});
</script>

<template>
  <section v-if="todosCompleted.length > 0" class="todo-list todo-done-list">
    <hr />
    <button class="task-btn" @click="showCompleted = !showCompleted">
      <h3>Completed ({{ todosCompleted.length }})</h3>
    </button>

    <div v-if="showCompleted" class="list">
      <div
        v-for="todo in todosCompleted"
        :key="todo.createdAt"
        class="todo-item"
        :class="{ done: todo.done }"
      >
        <label>
          <input type="checkbox" v-model="todo.done" />
          <span :class="`bubble ${todo.category}`"></span>
        </label>
        <div class="todo-content">
          <input type="text" v-model="todo.content" />
        </div>
        <div class="actions">
          <button class="delete" @click="removeTodo(todo)">Delete</button>
        </div>
      </div>
    </div>
  </section>
</template>
