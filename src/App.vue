<template>
  <div id="app">
    <Main></Main>
  </div>
</template>

<script>

import Main from './components/Main.vue'

export default {
  name: 'App',
  components: {
    Main
  }
}
</script>

<style lang="scss">
@import'~bootstrap/dist/css/bootstrap.css';
@import '~@fortawesome/fontawesome-free/css/all.min.css';

body{
  background: rgb(23,1,80);
  background: linear-gradient(90deg, rgba(23,1,80,1) 0%, rgba(76,0,88,1) 51%, rgba(20,0,70,1) 100%);
  // background-repeat: no-repeat;
  background-size: cover;
  max-height: 100vh;
}

</style>
