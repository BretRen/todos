<script setup lang="ts">
import { RouterView } from 'vue-router'
import DefaultLayout from './layout/DefaultLayout.vue';
</script>

<template>
  <DefaultLayout>
    <router-view />
  </DefaultLayout>
</template>