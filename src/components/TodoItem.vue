<script setup lang="ts">
import { PhTrash } from '@phosphor-icons/vue';

export interface Todo {
  id: string;
  done: boolean;
  description: string;
}

interface Props {
  todo: Todo;
}

defineProps<Props>();

const emit = defineEmits<{
  (event: 'onDeleteTodo'): void;
  (event: 'onToggleDone'): void;
}>();
</script>

<template>
  <div class="todo-item">
    <label :htmlFor="todo.id" class="custom-checkbox">
      <input
        type="checkbox"
        :checked="todo.done"
        :name="todo.id"
        :id="todo.id"
        @change="emit('onToggleDone')"
      />
    </label>

    <p class="todo-text">{{ todo.description }}</p>

    <button class="remove-todo" title="Excluir tarefa" @click="emit('onDeleteTodo')">
      <PhTrash :size="22" />
    </button>
  </div>
</template>

<style scoped lang="scss">
.todo-item {
  display: flex;
  align-items: flex-start;
  gap: 0.75rem;
  padding: 1rem;

  background-color: var(--gray-500);
  border-radius: 8px;

  .custom-checkbox {
    line-height: 0;

    > input[type='checkbox'] {
      -webkit-appearance: none;
      appearance: none;
      cursor: pointer;
      width: 18px;
      height: 18px;
      border: 2px solid var(--blue);
      border-radius: 50%;
      background-color: transparent;

      display: flex;
      align-items: center;
      justify-content: center;

      &:checked {
        background-color: var(--purple-dark);
        border: 2px solid var(--purple-dark);

        &::before {
          content: '';
          border: solid var(--gray-100);
          border-width: 0 2px 2px 0;
          width: 3px;
          height: 7px;
          -webkit-transform: rotate(45deg) translate(-1px, -1px);
          -ms-transform: rotate(45deg) translate(-1px, -1px);
          transform: rotate(45deg) translate(-1px, -1px);
        }
      }
    }
  }

  .todo-text {
    flex: 1;
    color: var(--gray-100);
  }

  .remove-todo {
    line-height: 0;
    background: none;
    border: none;
    color: var(--gray-300);

    transition: 0.2s color ease-in-out;

    &:hover {
      color: var(--danger);
    }
  }
}
</style>
