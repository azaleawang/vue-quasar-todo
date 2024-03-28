<script setup lang="ts">
import { ref, type ModelRef } from 'vue';
import type { Todo } from './models';

const todos = defineModel('todos') as ModelRef<Todo[]>;
const todoCategory = defineModel('category') as ModelRef<string>;
const props = defineProps(['selectedCategory']);
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
    console.log(newTodo);
  }
};

const resetForm = () => {
  todoContent.value = '';
  todoCategory.value = 'work';
};
</script>
<template>
  <div class="col-8 col-md-9">
    <q-input
      class="q-ma-lg text-body1"
      rounded
      outlined
      v-model="todoContent"
      placeholder="Add new task"
      required
      icon="task"
      @keyup.enter="addTodo"
    >
      <q-btn rounded flat icon="add" @click="addTodo" />
    </q-input>
  </div>
</template>
<style lang="sass"></style>
