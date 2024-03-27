<template>
  <q-page class="column bg-grey-2 q-pa-md">
    <main class="app container">
      <div class="app-container">
        <UsernameInput v-model:name="username" />
        <div class="d-flex flex-column mx-4">
          <select class="form-select my-2" v-model="selectedCategory">
            <option v-for="category in categoryList" :value="category" :key="category">
              {{ category }}
            </option>
          </select>
          <h3 class="p-1">
            <button
              class="text-secondary mb-3"
              @click="addNewCategory = !addNewCategory"
            >
              ... OR add a new category
            </button>
            <div
              class="mb-3 d-flex align-items-center"
              :class="addNewCategory ? '' : 'd-none'"
            >
              <input
                type="text"
                v-model="newCategory"
                class="form-control me-2 rounded-0 border-bottom border-dark"
                placeholder="New category"
              />
              <button
                class="btn btn-secondary btn-sm rounded-pill"
                @click="addCategory"
              >
                Add
              </button>
            </div>
          </h3>
        </div>
        <div class="todo-container">
          <TodoForm
            v-model:todos="todos"
            v-model:category="todoCategory"
            :selectedCategory="selectedCategory"
          />
          <TodoList
            v-model:todos="todos"
            :selectedCategory="selectedCategory"
          />
        </div>
      </div>
    </main>
  </q-page>
</template>

<script setup lang="ts">
import { ref, onMounted, watch } from 'vue';
import type { Todo } from '../components/models';
import UsernameInput from '../components/UsernameInput.vue';
import TodoForm from '../components/TodoForm.vue';
import TodoList from '../components/TodoList/TodoListIndex.vue';
const todos = ref<Todo[]>([]);
const username = ref('');
const todoCategory = ref('work');
const categoryList = ref<string[]>(['work', 'personal']);
const selectedCategory = ref<string>('work');
const newCategory = ref('');
const addNewCategory = ref(false);

const addCategory = () => {
  if (newCategory.value && !categoryList.value.includes(newCategory.value)) {
    categoryList.value.push(newCategory.value);
    selectedCategory.value = newCategory.value;
    addNewCategory.value = false;
    newCategory.value = '';
  }
};

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

watch(
  categoryList,
  (newCategoryList) => {
    localStorage.setItem('categoryList', JSON.stringify(newCategoryList));
  },
  {
    deep: true,
  }
);

onMounted(() => {
  const storedUsername = localStorage.getItem('username');
  const storedTodos = localStorage.getItem('todos');
  const storedCategoryList = localStorage.getItem('categoryList');

  username.value = storedUsername || '';
  todos.value = storedTodos ? JSON.parse(storedTodos) : [];
  if (storedCategoryList) {
    categoryList.value = JSON.parse(storedCategoryList);
  } else {
    localStorage.setItem('categoryList', JSON.stringify(categoryList.value));
  }
});
</script>
