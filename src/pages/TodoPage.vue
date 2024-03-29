<template>
  <q-page class="q-pa-md column items-center">
    <main class="col" style="width: 100%; max-width: 800px">
      <p class="text-h3 text-weight-bold q-ma-lg ">My Task</p>
      <!-- <UsernameInput v-model:name="username" /> -->
      <!-- <div>
        <select class="form-select my-2" v-model="selectedCategory">
          <option
            v-for="category in categoryList"
            :value="category"
            :key="category"
          >
            {{ category }}
          </option>
        </select>
        <h3>
        <button @click="addNewCategory = !addNewCategory">
          ... OR add a new category
        </button>
        <div
          class="mb-3 d-flex align-items-center"
          :class="addNewCategory ? '' : 'd-none'"
        >
          <input type="text" v-model="newCategory" placeholder="New category" />
          <button @click="addCategory">Add</button>
        </div>
      </h3>
      </div> -->
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
      <TodoList v-model:todos="todos" :selectedCategory="selectedCategory" />
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
    const parsedCategoryList = JSON.parse(storedCategoryList);
    console.log(parsedCategoryList); // it print "personal", why?
    categoryList.value = JSON.parse(storedCategoryList);
  } else {
    localStorage.setItem('categoryList', JSON.stringify(categoryList.value));
  }
});
</script>
