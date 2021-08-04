<template>
  <navbar></navbar>
  <div class="app">
    <div class="container">
    <router-view></router-view>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar"
export default {
  components: {
    Navbar
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif;
}
.app {
  padding: 1.5rem;
}
.container {
  max-width: 1140px;
  margin: auto;
}
.btn {
  align-self: flex-end;
  padding: .6rem 1rem;
  background: none;
  border: 1px solid;
  color: #1b41b3;
  border-radius: .25rem;
}
</style>