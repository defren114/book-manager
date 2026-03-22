<template>
  <form @submit.prevent="handleSubmit" class="card p-4 mb-4">
    <h4 class="mb-3">Добавить новую книгу</h4>
    <div class="mb-3">
      <input 
        v-model="formData.title" 
        type="text" 
        class="form-control" 
        placeholder="Название книги" 
        required 
      />
    </div>
    <div class="mb-3">
      <input 
        v-model="formData.author" 
        type="text" 
        class="form-control" 
        placeholder="Автор" 
        required 
      />
    </div>
    <div class="mb-3">
      <input 
        v-model="formData.cover" 
        type="url" 
        class="form-control" 
        placeholder="Ссылка на обложку (URL)" 
      />
    </div>
    <div class="mb-3">
      <textarea 
        v-model="formData.description" 
        class="form-control" 
        placeholder="Описание книги" 
        rows="3"
      ></textarea>
    </div>
    <div class="mb-3">
      <select v-model="formData.genre" class="form-select" required>
        <option value="">Выберите жанр</option>
        <option value="Роман">Роман</option>
        <option value="Фантастика">Фантастика</option>
        <option value="Детектив">Детектив</option>
        <option value="Научная">Научная</option>
        <option value="Поэзия">Поэзия</option>
      </select>
    </div>
    <button type="submit" class="btn btn-success w-100">Добавить книгу</button>
  </form>
</template>

<script setup>
import { reactive } from 'vue'
const emit = defineEmits(['add-book'])
const formData = reactive({
  title: '',
  author: '',
  genre: '',
  cover: '',
  description: ''
})
const handleSubmit = () => {
  emit('add-book', { ...formData })
  formData.title = ''
  formData.author = ''
  formData.genre = ''
  formData.cover = ''
  formData.description = ''
}
</script>