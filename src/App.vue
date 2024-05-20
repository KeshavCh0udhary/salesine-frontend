<template>
  <NavBar />
  <router-view/>
</template>

<script>
  import NavBar from './components/NavBar';
  export default{
    components: { NavBar }
}
</script>

<style>
  *{
    font-family: 'Poppins', sans-serif;
  }
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

</style>
