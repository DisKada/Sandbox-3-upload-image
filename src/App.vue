<template>
  <div id="app">
    <div class="container d-flex justify-content-center align-items-center">
      <div class="card shadow-lg p-5">
        <Upload />
      </div>
    </div>
  </div>
</template>

<script>
import Upload from './components/Upload.vue'

export default {
  name: 'App',
  components: {
    Upload
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  min-height: 80vh;
}

.container {
  height: 80vh;
}
</style>
