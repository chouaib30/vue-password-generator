<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <h1 class="mb-3 mt-2">Vue Password Generator</h1>
    <PasswordGenerator />
  </div>
</template>

<script>
import PasswordGenerator from './components/PasswordGenerator.vue'

export default {
  name: 'App',
  components: {
    PasswordGenerator
  }
}
</script>

<style lang="scss">
  @import url('https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.4.1/css/bootstrap.min.css');
  @import url('https://fonts.googleapis.com/css?family=Raleway:400,700&display=swap');
  
  *{
    margin:0;
    padding:0;
    box-sizing:border-box;
  }

  body{
    background: #f8f9fa;
    color: #343a40;
    font-size: 18px;
    display: flex;
    justify-content: center;
    align-items:center;
    height:100vh;
    width:100%;
  }

  #app {
    font-family: 'Raleway', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

  .vue-range-slider.slider-component .slider .slider-process {
    background-color:#28a745;
  }

  .vue-range-slider.slider-component .slider-tooltip-wrap .slider-tooltip {
    border: 1px solid#28a745;
    background-color:#28a745;
  }

  img{
    width: 150px;
  }

  h1{
    font-weight: 700;
  }
</style>
