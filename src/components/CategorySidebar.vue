<template>
  <q-item to="/" clickable v-ripple @click="filteredCategory = categoryList">
    <q-item-section avatar>
      <q-icon name="task_alt" color="secondary" />
    </q-item-section>
    <q-item-section> Task </q-item-section>
  </q-item>

  <q-expansion-item
    icon="category"
    header-class="text-secondary"
    label="Category"
  >
    <q-card>
      <q-item
        tag="label"
        v-for="category in categoryList"
        :key="category"
        v-ripple
        class="text-secondary q-px-lg q-py-none"
      >
        <q-item-section avatar>
          <q-checkbox :val="category" v-model="filteredCategory" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ category }}</q-item-label>
        </q-item-section>
      </q-item>
      <!-- Add a new category -->
      <q-item tag="label" v-ripple class="text-secondary q-px-lg q-py-none">
        <q-item-section avatar>
          <q-btn icon="add" flat round @click="addCategory" />
        </q-item-section>
        <q-input
          v-model="newCategory"
          @keyup.enter="addCategory"
          dense
          required
          placeholder="New"
        />
      </q-item>
    </q-card>
  </q-expansion-item>
</template>

<script setup lang="ts">
import { ref, type ModelRef } from 'vue';

const newCategory = ref('');
const categoryList = defineModel('category') as ModelRef<string[]>;
const filteredCategory = defineModel('filteredCategory') as ModelRef<string[]>;

const addCategory = () => {
  if (newCategory.value && !categoryList.value.includes(newCategory.value)) {
    categoryList.value.push(newCategory.value);
    newCategory.value = '';
  }
};

</script>
