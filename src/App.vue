<template>
  <div id="app">
    <SearchWindow></SearchWindow>
    <div class="start-bar">
      <div class="start-bar-button">
        <img src="./assets/start.png">
      </div>
    </div>
  </div>
</template>

<script>

  import '98.css'
  import SearchWindow from './components/SearchWindow.vue';

  export default {
    name: 'App',
    components: { SearchWindow }
  }

</script>

<style>

  * {
    box-sizing: border-box;
  }

  body {
    margin: 0px;
  }

  #app {
    height: 100vh;
    max-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #338081;
  }

  .start-bar {
    padding: 2px;
    box-shadow: inset 1px 0 #fff;
    display: block;
    background-color: silver;
    bottom: 0;
    left: 0;
    right: 0;
    border-top: 1px solid #f4f4f4;
    border-bottom: 1px solid #4e4e4e;
    position: fixed;
    z-index: 99999;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
  }

  .start-bar .start-bar-button {
    display: inline-block;
    padding: 4px 6px 1px;
    background-color: silver;
    border-top: 1px solid #fff;
    border-left: 1px solid #fff;
    border-right: 1px solid gray;
    border-bottom: 1px solid gray;
    box-shadow: inset 1px 1px #dfdfdf, 1px 0 #000, 0 1px #000, 1px 1px #000;
    margin-right: 6px;
    color: #000;
  }

  .start-bar .start-bar-button img {
    width: 45px;
    height: 14px;
  }



</style>
