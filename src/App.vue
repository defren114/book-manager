<template>
  <div class="container py-5">
    <header class="text-center mb-5">
      <h1 class="display-4">📚 Менеджер книг</h1>
      <p class="lead text-muted">Управляй своей библиотекой</p>
    </header>
    
    <main>
      <AddBookForm @add-book="addBook" />
      
      <BookFilters
        v-model:searchQuery="searchQuery"
        v-model:filter="currentFilter"
        v-model:sortBy="sortBy"
        :books="books"
      />
      
      <div v-if="filteredBooks.length === 0" class="text-center py-5">
        <p class="display-1">📖</p>
        <p class="lead text-muted">Книги не найдены</p>
        <p class="text-muted">Добавьте первую книгу или измените параметры поиска</p>
      </div>
      
      <div v-else>
        <BookCard
          v-for="book in filteredBooks"
          :key="book.id"
          :book="book"
          @toggle="toggleBook(book.id)"
          @delete="deleteBook(book.id)"
          @rate="rateBook(book.id, $event)"
        />
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import AddBookForm from './components/AddBookForm.vue'
import BookFilters from './components/BookFilters.vue'
import BookCard from './components/BookCard.vue'

const books = ref([])
const savedBooks = localStorage.getItem('books')
if (savedBooks) {
  books.value = JSON.parse(savedBooks)
}

const currentFilter = ref('all')
const searchQuery = ref('')
const sortBy = ref('date')

watch(books, (newBooks) => {
  localStorage.setItem('books', JSON.stringify(newBooks))
}, { deep: true })

const addBook = (bookData) => {
  const newBook = {
    id: Date.now(),
    ...bookData,
    completed: false,
    rating: 0,
    dateAdded: new Date().toISOString()
  }
  books.value.push(newBook)
}

const toggleBook = (id) => {
  const book = books.value.find(b => b.id === id)
  if (book) {
    book.completed = !book.completed
    if (!book.completed) {
      book.rating = 0
    }
  }
}

const rateBook = (id, rating) => {
  const book = books.value.find(b => b.id === id)
  if (book && book.completed) {
    book.rating = rating
  }
}

const deleteBook = (id) => {
  if (confirm('Удалить книгу?')) {
    books.value = books.value.filter(b => b.id !== id)
  }
}

const filteredBooks = computed(() => {
  let result = books.value
    .filter(book => {
      if (currentFilter.value === 'unread') return !book.completed
      if (currentFilter.value === 'read') return book.completed
      return true
    })
    .filter(book => {
      if (!searchQuery.value) return true
      const query = searchQuery.value.toLowerCase()
      return book.title.toLowerCase().includes(query) ||
             book.author.toLowerCase().includes(query)
    })
  
  if (sortBy.value === 'title') {
    result.sort((a, b) => a.title.localeCompare(b.title))
  } else if (sortBy.value === 'author') {
    result.sort((a, b) => a.author.localeCompare(b.author))
  } else {
    result.sort((a, b) => b.id - a.id)
  }
  
  return result
})
</script>

<style>
body {
  background: #f8f9fa;
}
</style>