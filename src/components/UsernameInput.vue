<script setup lang="ts">
const name = defineModel('name')
</script>

<template>
  <section class="greeting">
    <h2 class="title">
      Hi,
      <input
        type="text"
        placeholder="Name here"
        v-model="name" 
      />
    </h2>
  </section>
</template>
