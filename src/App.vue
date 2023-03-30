<script setup lang="ts">
import { ref, computed } from 'vue';
import { v4 as uuidv4 } from 'uuid';

import NavBar from '@/components/NavBar.vue';
import EmptyState from '@/components/EmptyState.vue';
import TodoItem from '@/components/TodoItem.vue';
import type { Todo } from '@/components/TodoItem.vue';

import { PhPlusCircle } from '@phosphor-icons/vue';

const todoList = ref<Todo[]>([]);
const todoInput = ref<string>('');

const doneTodoCount = computed(() =>
  todoList.value.reduce((acc, todo) => {
    if (todo.done) acc += 1;

    return acc;
  }, 0)
);

const handleCreateTodo = () => {
  if (!todoInput.value) return;

  todoList.value.unshift({
    id: uuidv4(),
    description: todoInput.value,
    done: false,
  });

  todoInput.value = '';
};

const handleToggleTodoAsDone = (id: string) => {
  const todoIndex = todoList.value.findIndex((todo) => todo.id === id);

  if (todoIndex !== -1) {
    todoList.value[todoIndex].done = !todoList.value[todoIndex].done;
  }
};

const handleDeleteTodo = (id: string) => {
  todoList.value = todoList.value.filter((todo) => todo.id !== id);
};
</script>

<template>
  <div>
    <NavBar />

    <main class="container">
      <form class="todo-form" @submit.prevent="handleCreateTodo">
        <input v-model="todoInput" type="text" placeholder="Adicione uma nova tarefa" required />

        <button type="submit" :disabled="!todoInput">
          Criar <PhPlusCircle :size="16" weight="bold" />
        </button>
      </form>

      <section class="todo-content">
        <header>
          <p class="created-todos">
            Tarefas Criadas <span>{{ todoList.length }}</span>
          </p>
          <p class="completed-todos">
            Concluídas
            <span>{{ !todoList.length ? 0 : `${doneTodoCount} de ${todoList.length}` }}</span>
          </p>
        </header>

        <div v-if="todoList.length" class="todo-list-wrapper">
          <TodoItem
            v-for="todo in todoList"
            :key="todo.id"
            :todo="todo"
            @on-toggle-done="() => handleToggleTodoAsDone(todo.id)"
            @on-delete-todo="() => handleDeleteTodo(todo.id)"
          />
        </div>
        <EmptyState v-else />
      </section>
    </main>
  </div>
</template>

<style scoped lang="scss">
.container {
  max-width: 46rem;
  margin: 0 auto;
  padding: 0 1rem;
}

.todo-form {
  display: flex;
  gap: 0.5rem;

  transform: translateY(-50%);

  > input {
    flex: 1;

    background-color: var(--gray-500);
    border: 1px solid var(--gray-700);
    border-radius: 8px;

    font-size: 1rem;
    color: var(--gray-300);
    line-height: 1.4rem;
    padding: 1rem;
  }

  > button[type='submit'] {
    background-color: var(--blue-dark);
    color: var(--gray-100);
    border-radius: 8px;
    border: none;

    display: flex;
    align-items: center;
    gap: 0.5rem;

    line-height: 0;
    font-size: 0.875rem;
    font-weight: 700;
    padding: 1rem;

    transition: 0.2s filter ease-in-out;

    &:hover {
      filter: brightness(0.8);
    }

    &:disabled {
      opacity: 0.7;
      pointer-events: none;
      cursor: not-allowed;
    }
  }
}

.todo-content {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;

  > header {
    display: flex;
    justify-content: space-between;

    > p {
      display: flex;
      align-items: center;
      gap: 0.5rem;

      font-weight: 700;
      font-size: 0.75rem;

      &.created-todos {
        color: var(--blue);
      }

      &.completed-todos {
        color: var(--purple);
      }

      > span {
        background-color: var(--gray-400);
        color: var(--gray-200);
        border-radius: 999px;
        padding: 0.125rem 0.5rem;
      }
    }
  }

  .todo-list-wrapper {
    max-height: calc(100vh - 12.5rem - 8.25rem);
    overflow-y: auto;

    div + div {
      margin-top: 0.75rem;
    }
  }
}
</style>
