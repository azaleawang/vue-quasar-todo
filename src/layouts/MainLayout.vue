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
          <q-item
            to="/"
            active
            clickable
            v-ripple
            @click="filteredCategory = categoryList"
          >
            <q-item-section avatar>
              <q-icon name="task_alt" color="secondary" />
            </q-item-section>

            <q-item-section> All </q-item-section>
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
                  <q-checkbox
                    v-model="filteredCategory"
                    :val="category"
                    active
                  />
                </q-item-section>
                <q-item-section>
                  <q-item-label>{{ category }}</q-item-label>
                </q-item-section>
              </q-item>
              <!-- Add a new category -->
              <q-item
                tag="label"
                v-ripple
                class="text-secondary q-px-lg q-py-none"
              >
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
      <router-view
        :filteredCategory="filteredCategory"
        :categoryList="categoryList"
      />
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
import { ref, watch, onMounted } from 'vue';

defineOptions({
  name: 'MainLayout',
});

const leftDrawerOpen = ref(false);
const newCategory = ref('');
const categoryList = ref<string[]>(['work', 'personal']);
const filteredCategory = ref(categoryList.value);

const toggleLeftDrawer = () => {
  leftDrawerOpen.value = !leftDrawerOpen.value;
};

const addCategory = () => {
  if (newCategory.value && !categoryList.value.includes(newCategory.value)) {
    categoryList.value.push(newCategory.value);
    newCategory.value = '';
  }
};

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
  const storedCategoryList = localStorage.getItem('categoryList');
  if (storedCategoryList) {
    const parsedCategoryList = JSON.parse(storedCategoryList);
    console.log(parsedCategoryList);
    categoryList.value = JSON.parse(storedCategoryList);
  } else {
    localStorage.setItem('categoryList', JSON.stringify(categoryList.value));
  }
});
</script>
