<template>
  <div id="app">
    <Main></Main>
    <router-view/>
  </div>
</template>

<style>
@import "assets/css/style.css";
</style>

<script>
import Main from "./components/main"
export default {
  components: {Main}
}
</script>