<template>
  <q-page class="q-pa-md column items-center">
    <main class="col" style="width: 100%; max-width: 800px">
      <p class="text-h5 text-weight-bold q-ma-lg">My Task</p>
      <div class="row items-center q-col-gutter-md q-px-sm">
        <q-select
          rounded
          outlined
          v-model="selectedCategory"
          :options="categories"
          class="col-12 col-sm-4"
        >
          <template v-slot:prepend>
            <q-icon name="category" color="secondary" />
          </template>
        </q-select>
        <TodoForm
          class="col-12 col-sm-8"
          v-model:todos="todos"
          v-model:category="todoCategory"
          :selectedCategory="selectedCategory"
        />
      </div>
      <TodoList v-model:todos="todos" :filteredCategory="filteredCategory" />
    </main>
  </q-page>
</template>

<script setup lang="ts">
import { ref, onMounted, watch, inject, type Ref } from 'vue';
import type { Todo } from '../components/models';
import TodoForm from '../components/TodoForm.vue';
import TodoList from '../components/TodoList/TodoListIndex.vue';

defineProps<{
  filteredCategory: string[]
  categoryList?: string[]
}>()
const todos = ref<Todo[]>([]);
const todoCategory = ref('work');
const selectedCategory = ref<string>('work');
const categories = inject('categoryList') as Ref<string[]>;

watch(
  todos,
  (newTodos) => {
    localStorage.setItem('todos', JSON.stringify(newTodos));
  },
  {
    deep: true,
  }
);

onMounted(() => {
  const storedTodos = localStorage.getItem('todos');
  todos.value = storedTodos ? JSON.parse(storedTodos) : [];
});
</script>
