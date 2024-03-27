<script setup lang="ts">
import type { Todo } from '../models';
import { ref, computed, type ModelRef } from 'vue';

const todos = defineModel('todos') as ModelRef<Todo[]>;
const showInprogress = ref(true);

const todosInprogress = computed(() => {
  return todos.value
    .filter((todo) => !todo.done)
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
  <section v-if="todosInprogress.length > 0" class="todo-list">
    <hr />
    <button class="task-btn" @click="showInprogress = !showInprogress">
      <h3>Tasks ({{ todosInprogress.length }})</h3>
    </button>
    <div class="list" v-if="showInprogress">
      <div
        v-for="todo in todosInprogress"
        :key="todo.createdAt"
        :class="`todo-item ${todo.done && 'done'}`"
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
  <section v-else class="todo-empty">
    <h3>No tasks yet</h3>
  </section>
</template>
