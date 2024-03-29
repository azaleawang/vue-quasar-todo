<template>
  <q-page class="q-pa-md column items-center">
    <main class="col" style="width: 100%; max-width: 800px">
      <p class="text-h5 text-weight-bold q-ma-lg">My Task</p>
      <div class="row items-center q-col-gutter-md q-px-sm">
        <q-select
          rounded
          outlined
          v-model="selectedCategory"
          :options="categoryList"
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
import { ref, onMounted, watch } from 'vue';
import type { Todo } from '../components/models';
import TodoForm from '../components/TodoForm.vue';
import TodoList from '../components/TodoList/TodoListIndex.vue';
const todos = ref<Todo[]>([]);
const username = ref('');
const todoCategory = ref('work');
const props = defineProps(['filteredCategory', 'categoryList']);
const categories = ref(props.categoryList);
// const categoryList = ref<string[]>(['work', 'personal']);
const selectedCategory = ref<string>('work');

watch(username, (newName) => {
  localStorage.setItem('username', newName);
});

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
  const storedCategoryList = localStorage.getItem('categoryList');
  todos.value = storedTodos ? JSON.parse(storedTodos) : [];
  if (storedCategoryList) {
    const parsedCategoryList = JSON.parse(storedCategoryList);
    console.log(parsedCategoryList);
    categories.value = JSON.parse(storedCategoryList);
  } else {
    localStorage.setItem('categoryList', JSON.stringify(categories.value));
  }
});
</script>
