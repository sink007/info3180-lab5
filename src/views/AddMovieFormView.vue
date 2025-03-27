<template>
    <div class="add-container">
      <h1>Add a New Movie</h1>
      <MovieForm />
    </div>
</template>
  
<script>
    import MovieForm from '../components/MovieForm.vue';

    export default {
    name: 'AddMovieFormView',
    components: {
        MovieForm
    },
    };
</script>
  
<style scoped>
    .add-container {
    max-width: 600px;
    margin: auto;
    padding: 20px;
    }
</style>