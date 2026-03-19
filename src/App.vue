<script setup lang="ts">
import AppMain from './components/AppMain.vue'
import AppSidebar from './components/AppSidebar.vue'
</script>

<template>
  <div class="min-h-screen grid grid-cols-[267px_1fr]">
    <AppSidebar />
    <AppMain />
  </div>
</template>
