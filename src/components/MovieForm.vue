<template>
    <div class="container mt-4">
      <form id="movieForm" @submit.prevent="saveMovie">
        <div class="form-group mb-3">
          <label for="title" class="form-label">Movie Title</label>
          <input v-model="title" type="text" name="title" class="form-control"/>
        </div>
  
        <div class="form-group mb-3">
          <label for="description" class="form-label">Description</label>
          <textarea v-model="description" name="description" class="form-control"></textarea>
        </div>
  
        <div class="form-group mb-3">
          <label for="poster" class="form-label">Upload Poster</label>
          <input type="file" @change="handleFileUpload" class="form-control" />
        </div>
  
        <button type="submit" class="btn btn-primary">Submit</button>
      </form>
    </div>
  </template>
  
<script setup>
    import { ref, onMounted } from 'vue';
    
    const title = ref('');
    const description = ref('');
    const poster = ref(null);
    let csrf_token = ref("");

    function getCsrfToken() {
        fetch('/api/v1/csrf-token')
        .then((response) => response.json())
        .then((data) => {
         console.log(data);
         csrf_token.value = data.csrf_token;
        })
    }

    onMounted(() => {
        getCsrfToken();
    });

    const handleFileUpload = (event) => {
        poster.value = event.target.files[0];
    };
    
    const saveMovie = async () => {
        let movieForm = document.getElementById('movieForm');
        let form_data = new FormData(movieForm);
        form_data.append('title', title.value);
        form_data.append('description', description.value);
        form_data.append('poster', poster.value);
        console.log(form_data);    
        try {
        let response = await fetch('/api/v1/movies', {
            method: 'POST',
            body: form_data,
            headers: {
                'X-CSRFToken': csrf_token.value
            }
        });
    
        let data = await response.json();
        console.log(data);
        } catch (error) {
        console.error('Error:', error);
        }
    };
    </script>
    
    <style scoped>
    .container {
        max-width: 600px;
        margin: auto;
    }
    </style>
  