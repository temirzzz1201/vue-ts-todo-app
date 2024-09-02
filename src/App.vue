<template>
  <app-header />
  <app-filters :active-filter="activeFilter" @set-filter="setFilterHandler" />
  <main class="app-main">
    <app-todo-list :todos="filteredTodos" @remove-todo="removeTodoHandler" @toggle-todo="toggleTodoHandler"/>
    <app-add-todo @add-todo="addTodoHandler" />
  </main>
  <app-footer :stats="stats" />
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import AppHeader from '@/components/AppHeader.vue'
import AppFilters from '@/components/AppFilters.vue'
import AppTodoList from '@/components/AppTodoList.vue'
import AppAddTodo from '@/components/AppAddTodo.vue'
import AppFooter, { Stats } from '@/components/AppFooter.vue'
import { ITodo } from './types/todo'
import { TFilter } from './types/filter'

interface State {
  todos: ITodo[],
  activeFilter: TFilter
}

export default defineComponent({
  components: {
    AppHeader,
    AppFilters,
    AppTodoList,
    AppAddTodo,
    AppFooter
  },
  data(): State {
    return {
      todos: [
        { id: 1, text: 'Изучить новый ЯП..', isCompleted: true },
        { id: 2, text: 'Подтянуть Typescript', isCompleted: false },
        { id: 3, text: 'Покормить Ваську)', isCompleted: false },
      ],
      activeFilter: 'Все'
    }
  },
  computed: {
    filteredTodos(): ITodo[] {
      switch (this.activeFilter) {
        case 'Активные':
          return this.activeTodods
        case 'Завершенные':
          return this.doneTodos
        case 'Все':
        default:
          return this.todos
      }
    },
    stats(): Stats {
      return {
        active: this.activeTodods.length,
        done: this.doneTodos.length
      }
    },
    activeTodods(): ITodo[] {
      return this.todos.filter(todo => !todo.isCompleted)
    },
    doneTodos(): ITodo[] {
      return this.todos.filter(todo => todo.isCompleted)
    }
  },
  methods: {
    addTodoHandler(todo: ITodo) {
      this.todos.push(todo)
    },
    toggleTodoHandler(id: number) {
      const nessesaryTodo = this.todos.find((todo: ITodo) => todo.id === id)
      if(nessesaryTodo) nessesaryTodo.isCompleted = !nessesaryTodo.isCompleted
    },
    removeTodoHandler(id: number) {
      this.todos = this.todos.filter((todo: ITodo) => todo.id !== id)
    },
    setFilterHandler(filter: TFilter) {
      this.activeFilter = filter
    }
  }
})
</script>