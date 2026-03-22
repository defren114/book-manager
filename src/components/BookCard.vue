<template>
  <div class="card mb-3" :class="{ 'bg-success-subtle': book.completed }">
    <div class="row g-0">
      <div class="col-md-2">
        <img 
          :src="book.cover || 'https://via.placeholder.com/150x200?text=No+Cover'" 
          class="img-fluid rounded-start h-100" 
          style="object-fit: cover;"
          alt="Обложка книги"
        />
      </div>
      <div class="col-md-10">
        <div class="card-body">
          <h5 class="card-title">{{ book.title }}</h5>
          <p class="card-text text-muted">Автор: {{ book.author }}</p>
          <p class="card-text" v-if="book.description">{{ book.description }}</p>
          <span class="badge bg-secondary">{{ book.genre }}</span>
          
          <div class="mt-3" v-if="book.completed">
            <span class="text-warning fs-5">
              <span v-for="star in 5" :key="star" @click="$emit('rate', star)" style="cursor: pointer;">
                {{ star <= book.rating ? '★' : '☆' }}
              </span>
            </span>
            <span class="ms-2 text-muted">({{ book.rating }}/5)</span>
          </div>
          
          <div class="mt-3">
            <button 
              @click="$emit('toggle')" 
              :class="['btn', book.completed ? 'btn-outline-success' : 'btn-primary']"
            >
              {{ book.completed ? '✓ Прочитано' : 'Отметить прочитанной' }}
            </button>
            <button @click="$emit('delete')" class="btn btn-danger ms-2">✕ Удалить</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps(['book'])
defineEmits(['toggle', 'delete', 'rate'])
</script>