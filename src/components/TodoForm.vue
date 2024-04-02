<template>
  <div>
    <q-input
      class="text-body1"
      rounded
      outlined
      v-model="todoContent"
      placeholder="Add new task"
      required
      @keyup.enter="addTodo"
    >
      <q-btn rounded flat icon="add" @click="addTodo" />
    </q-input>
  </div>
</template>

<script setup lang="ts">
import { ref, type ModelRef } from 'vue';
import type { Todo } from './models';

const todos = defineModel('todos') as ModelRef<Todo[]>;
const todoCategory = defineModel('category') as ModelRef<string>;
const props = defineProps<{
  selectedCategory: string;
}>();
const todoContent = ref('');

const isValidInput = () =>
  todoContent.value.trim() !== '' && todoCategory.value !== '';

const addTodo = () => {
  if (isValidInput()) {
    const newTodo: Todo = {
      content: todoContent.value,
      category: props.selectedCategory,
      done: false,
      createdAt: Date.now(),
    };
    todos.value.push(newTodo);
    resetForm();
  }
};

const resetForm = () => {
  todoContent.value = '';
  todoCategory.value = 'work';
};
</script>
