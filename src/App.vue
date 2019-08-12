<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
html,body, #app{
  width:100%;
  height:100%;
  padding: 0;
  margin:0;
  border:0;
  font-size: 14px;
  font-style: normal;
  list-style: none;
  text-decoration: none;
  /* box-sizing: border-box; */
  overflow: hidden;
  outline:none;
}
</style>
