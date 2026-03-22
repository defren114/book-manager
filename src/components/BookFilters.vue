<template>
  <div class="card p-4 mb-4">
    <div class="mb-3">
      <input 
        v-model="searchQuery" 
        type="text" 
        class="form-control" 
        placeholder="🔍 Поиск по названию или автору..." 
      />
    </div>
    <div class="d-flex gap-2 mb-3">
      <button
        v-for="option in filterOptions"
        :key="option.value"
        @click="$emit('update:filter', option.value)"
        :class="['btn', filter === option.value ? 'btn-primary' : 'btn-outline-secondary']"
      >
        {{ option.label }}
      </button>
    </div>
    <div class="d-flex justify-content-between align-items-center">
      <p class="mb-0 text-muted">
        Всего: <strong>{{ total }}</strong> | 
        Прочитано: <strong>{{ completed }}</strong> | 
        Осталось: <strong>{{ total - completed }}</strong>
      </p>
      <select v-model="sortBy" @change="$emit('update:sortBy', sortBy)" class="form-select w-auto">
        <option value="date">По дате</option>
        <option value="title">По названию</option>
        <option value="author">По автору</option>
      </select>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
const props = defineProps(['filter', 'books'])
defineEmits(['update:filter', 'update:sortBy'])
const searchQuery = defineModel('searchQuery')
const sortBy = defineModel('sortBy')
const filterOptions = [
  { value: 'all', label: 'Все' },
  { value: 'unread', label: 'Непрочитанные' },
  { value: 'read', label: 'Прочитанные' }
]
const total = computed(() => props.books.length)
const completed = computed(() => props.books.filter(b => b.completed).length)
</script>