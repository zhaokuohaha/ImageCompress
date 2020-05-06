<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
  export default {
    name: 'image-compress'
  }
</script>

<style>
body{
  background: radial-gradient(
    ellipse at top left,
    rgba(255, 255, 255, 1) 40%,
    rgba(229, 229, 229, 0.9) 100%
  );
  min-height: 100vh;
  padding: 10px 16px;
  /* width: 100vw; */
}

label {
  cursor: unset;
}

a{
  color: rgb(22, 97, 171);
  text-decoration: none;
}

.button{
  cursor: pointer;
  border-radius: 4px;
  background-color: rgba(22, 97, 171, .2);
  border:1px solid rgb(22, 97, 171);
  padding: 8px;
}

.menu{
  text-align: right;
  /* padding: 0 16px; */
}
</style>
