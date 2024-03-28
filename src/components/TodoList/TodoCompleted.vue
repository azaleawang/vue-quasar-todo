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
  <section v-if="todosCompleted.length > 0">
    <q-expansion-item
      expand-separator
      icon="task_alt"
      :label="`Completed (${todosCompleted.length})`"
      header-class="text-grey-7 text-subtitle1"
    >
      <q-list class="bg-white" separator>
        <q-item
          v-for="todo in todosCompleted"
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
          <q-item-section>
            <input
              class="text-body1 todo-content"
              v-model="todo.content"
              style="outline: none; border: 0"
            />
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
    </q-expansion-item>
    <div v-if="showCompleted" class="list"></div>
  </section>
</template>
