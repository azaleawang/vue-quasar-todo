<script setup lang="ts">
import { ref, type ModelRef } from 'vue';
import type { Todo } from './models'

const todos = defineModel('todos') as ModelRef<Todo[]>;
const todoCategory = defineModel('category') as ModelRef<string>;
const props = defineProps(['selectedCategory'])
const todoContent = ref("");

const isValidInput = () => todoContent.value.trim() !== "" && todoCategory.value !== "";

const addTodo = () => {
  if (isValidInput()) {
    const newTodo: Todo = {
      content: todoContent.value,
      category: props.selectedCategory,
      done: false,
      createdAt: Date.now()
    }
    todos.value.push(newTodo)
    console.log(newTodo);

    resetForm()
  }
}

const resetForm = () => {
  todoContent.value = ""
  todoCategory.value = "work"
}
</script>
<template>
  <section class="create-todo">
    <form @submit.prevent="addTodo">
      <h4>Add a task</h4>
      <input type="text" placeholder="Title" v-model="todoContent" required />
      <!-- <h4>Pick a category</h4>
      <div class="options">
        <label>
          <input
            type="radio"
            name="category"
            id="category1"
            value="work"
            v-model="todoCategory"
          />
          <span class="bubble work"></span>
          <div>Work</div>
        </label>
        <label>
          <input
            type="radio"
            name="category"
            id="category2"
            value="personal"
            v-model="todoCategory"
          />
          <span class="bubble personal"></span>
          <div>Personal</div>
        </label>
      </div> -->
      <button type="submit" class="add-btn">Add todo</button>
    </form>
  </section>
</template>

<style scoped>
.add-btn {
  display: none;
}
</style>
