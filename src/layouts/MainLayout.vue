<template>
  <q-layout view="hHh lpr fff">
    <q-header elevated>
      <q-toolbar>
        <q-btn flat @click="toggleLeftDrawer" round dense icon="menu" />
        <!-- <q-toolbar-title></q-toolbar-title> -->
      </q-toolbar>
      <div class="q-mt-lg q-pb-lg q-mx-xl">
        <div class="text-h3 q-mb-md">Tasks</div>
        <div class="text-subtitle1">{{ todayDate }}</div>
      </div>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      :width="200"
      :breakpoint="640"
    >
      <q-scroll-area class="fit">
        <q-list padding class="menu-list">
          <q-item to="/" active clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="task_alt" />
            </q-item-section>

            <q-item-section> All </q-item-section>
          </q-item>

          <q-item clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="star" />
            </q-item-section>

            <q-item-section> Star </q-item-section>
          </q-item>

          <q-item clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="list" />
            </q-item-section>
            <q-item-section> Category </q-item-section>
          </q-item>
          <q-item to="/about" clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="info" />
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
import { date } from 'quasar';
import { ref, computed } from 'vue';

defineOptions({
  name: 'MainLayout',
});
const timeStamp = Date.now();
const todayDate = computed(() => date.formatDate(timeStamp, 'YYYY-MM-DD ddd'));
const leftDrawerOpen = ref(false);
function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value;
}
</script>

<style lang="sass" scoped>
.menu-list .q-item
  border-radius: 0 32px 32px 0
</style>
