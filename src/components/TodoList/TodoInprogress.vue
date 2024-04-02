<template>
  <section>
    <q-expansion-item
      expand-separator
      icon="donut_large"
      :label="`In-progress (${todosInprogress.length})`"
      default-opened
      header-class="text-subtitle1"
    >
      <q-list v-if="todosInprogress.length" class="bg-white" separator>
        <q-item
          v-for="todo in todosInprogress"
          :key="todo.createdAt"
          :class="{ done: todo.done }"
          v-ripple
        >
          <q-item-section avatar>
            <q-checkbox
              v-model="todo.done"
              checked-icon="done"
              unchecked-icon="radio_button_unchecked"
              color="accent"
            />
          </q-item-section>
          <q-item-section class="todo-container">
            <input
              class="text-body1 todo-content"
              v-model="todo.content"
              style="outline: none; border: 0"
            />
          </q-item-section>
          <q-item-section avatar>
            <q-btn
              @click.stop="removeTodo(todo)"
              flat
              round
              color="secondary"
              icon="delete"
            />
          </q-item-section>
        </q-item>
      </q-list>
      <section v-else class="text-center">
        <q-img
          src="../../assets/task-done.png"
          spinner-color="white"
          style="width: 250px"
        ></q-img>
        <p class="text-subtitle1"></p>
      </section>
    </q-expansion-item>
  </section>
</template>

<script setup lang="ts">
import type { Todo } from '../models';
import { computed, type ModelRef } from 'vue';

defineProps({
  removeTodo: {
    type: Function,
    default: () => () => console.warn('removeTodo not provided'),
  },
});

const todos = defineModel('todos') as ModelRef<Todo[]>;

const todosInprogress = computed(() => {
  return todos.value
    .filter((todo) => !todo.done)
    .sort((a, b) => b.createdAt - a.createdAt);
});
</script>
