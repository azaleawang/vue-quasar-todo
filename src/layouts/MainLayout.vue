<template>
  <q-layout view="hHh Lpr fff">
    <q-header elevated>
      <q-toolbar>
        <q-btn flat @click="toggleLeftDrawer" round dense icon="menu" />
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      :width="200"
      :breakpoint="640"
    >
      <q-img class="absolute-top bg-accent" style="height: 150px">
        <div class="absolute-bottom bg-transparent">
          <q-avatar size="60px" class="q-mb-sm">
            <q-img
              ratio="1"
              fit="cover"
              src="https://www.catster.com/wp-content/uploads/2023/11/Beluga-Cat.webp"
            >
              <template v-slot:error>
                <div class="absolute-full flex flex-center"></div>
              </template>
            </q-img>
          </q-avatar>
          <div class="text-weight-bold">Azalea Wang</div>
          <div>@azalea.wang</div>
        </div>
      </q-img>
      <q-scroll-area
        style="
          height: calc(100% - 150px);
          margin-top: 150px;
          border-right: 1px solid #ddd;
        "
      >
        <q-list padding class="menu-list">
          <CategorySidebar
            v-model:category="categoryList"
            v-model:filteredCategory="filteredCategory"
          />
          <q-item to="/about" clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="info" color="secondary" />
            </q-item-section>
            <q-item-section> About </q-item-section>
          </q-item>
        </q-list>
      </q-scroll-area>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
import { ref, provide, onMounted, watch } from 'vue';
import CategorySidebar from '../components/CategorySidebar.vue';
defineOptions({
  name: 'MainLayout',
});
const categoryList = ref<string[]>(['work', 'personal']);
provide('categoryList', categoryList); // provide to TodoPage component

const filteredCategory = ref<string[]>(categoryList.value);
provide('filteredCategory', filteredCategory); // provide to TodoPage component

const leftDrawerOpen = ref(false);
const toggleLeftDrawer = () => {
  leftDrawerOpen.value = !leftDrawerOpen.value;
};

onMounted(() => {
  const storedCategoryList = localStorage.getItem('categoryList');
  if (storedCategoryList) {
    const parsedCategoryList = JSON.parse(storedCategoryList);
    console.log(parsedCategoryList);
    categoryList.value = JSON.parse(storedCategoryList);
  } else {
    localStorage.setItem('categoryList', JSON.stringify(categoryList.value));
  }
});

watch(
  categoryList,
  (newCategoryList) => {
    localStorage.setItem('categoryList', JSON.stringify(newCategoryList));
  },
  {
    deep: true,
  }
);
</script>
