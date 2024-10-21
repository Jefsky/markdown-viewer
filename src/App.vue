<template>
  <div id="app">
    <MarkdownViewer />
  </div>
</template>

<script>
import MarkdownViewer from './components/MarkdownViewer.vue';

export default {
  name: 'App',
  components: {
    MarkdownViewer
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
}
</style>
