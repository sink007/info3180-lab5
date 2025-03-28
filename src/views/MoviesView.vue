<script setup>
    import { ref, onMounted } from "vue";
    let movies = ref([]);

    onMounted(() => {
        fetchMovies();
    });

    const fetchMovies = async () => {
        try {
            const response = await fetch("/api/v1/movies"); 
            const data = await response.json();
            movies.value = data.movies;
        } catch (error) {
            console.error("Error fetching movies:", error);
        }
    };
</script>

<template>
    <div class="container mt-4">
      <h2 class="mb-3">Movies List</h2>
  
      <div v-if="movies.length" class="row">
        <div v-for="movie in movies" :key="movie.id" class="col-md-4 mb-4">
          <div class="card shadow-sm">
            <img :src="movie.poster" class="card-img-top" alt="Movie Poster" />
            <div class="card-body">
              <h5 class="card-title">{{ movie.title }}</h5>
              <p class="card-text">{{ movie.description }}</p>
            </div>
          </div>
        </div>
      </div>
  
      <p v-else>No movies found.</p>
    </div>
  </template>
  
  <style scoped>
  .card-img-top {
    height: 300px;
    object-fit: cover;
  }
  </style>