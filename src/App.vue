<template>
  <div id="page">
    <Books/>
  </div>
</template>

<script>
  import Books from './components/Books.vue'

export default {
  name: 'App',
  components: {
    Books
  }
}
</script>

<style>
  #page {
    background: #222629;
    color: white;
  }
</style>