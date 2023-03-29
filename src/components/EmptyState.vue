<script setup lang="ts">
import Clipboard from '@/assets/clipboard.svg?url';
</script>

<template>
  <div class="empty-state">
    <img :src="Clipboard" />
    <strong>Você ainda não tem tarefas cadastradas</strong>
    <p>Crie Tarefas e organize seus itens a fazer</p>
  </div>
</template>

<style scoped lang="scss">
.empty-state {
  border-top: 1px solid var(--gray-400);
  border-radius: 8px;
  padding: 4rem 0;

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  color: var(--gray-300);
  line-height: 1.4;

  > img {
    margin-bottom: 1rem;
  }
}
</style>
