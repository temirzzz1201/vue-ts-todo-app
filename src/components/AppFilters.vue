<template>
  <aside class="app-filters">
    <section class="toggle-group">
      <button class="button" v-for="item in filters" :key="item" :class="{ 'button--primary': activeFilter === item }" @click="filterHandler(item)">{{ item }}</button>
    </section>
  </aside>
</template>
  
<script lang="ts">
import { TFilter } from '@/types/filter';
import { PropType, defineComponent } from 'vue'

interface State {
  filters: TFilter[]
}

export default defineComponent({
  name: 'app-filters',
  data(): State {
    return {
      filters: ['Активные', 'Все', 'Завершенные']
    }
  },
  props: {
    activeFilter: {
      type: String as PropType<TFilter>,
      required: true
    }
  },
  methods: {
    filterHandler(filter: TFilter) {
      this.$emit('setFilter', filter)
    }
  },
  emits: {
    setFilter: (filter: TFilter) => filter
  }
})
</script>
