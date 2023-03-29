<script setup lang="ts">
import TodoLogo from '@/assets/todo-logo.svg?component';
</script>

<template>
  <header class="navbar">
    <TodoLogo />
  </header>
</template>

<style scoped lang="scss">
.navbar {
  background-color: var(--gray-700);
  height: 12.5rem;

  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
