<script setup lang="ts">
import type { Todo } from '../models';
import { computed, type ModelRef } from 'vue';

const todos = defineModel('todos') as ModelRef<Todo[]>;

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
  <q-page>
    <q-expansion-item
      expand-separator
      icon="donut_large"
      :label="`In-progress (${todosInprogress.length})`"
      default-opened
    >
      <q-list v-if="todosInprogress.length" class="bg-white" separator>
        <q-item
          v-for="todo in todosInprogress"
          :key="todo.createdAt"
          :class="{ done: todo.done }"
          v-ripple
          @click="todo.done = !todo.done"
          clickable
        >
          <q-item-section avatar>
            <q-checkbox
              v-model="todo.done"
              class="no-pointer-events"
              checked-icon="done"
              unchecked-icon="radio_button_unchecked"
              color="accent"
            />
          </q-item-section>
          <q-item-section>
            <q-item-label class="text-body1 todo-content">{{
              todo.content
            }}</q-item-label>
            <!-- <q-item-label caption>{{  }}</q-item-label> -->
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
      <section v-else class="todo-empty">
        <p class="text-subtitle1">No tasks yet</p>
      </section>
    </q-expansion-item>
  </q-page>
</template>
