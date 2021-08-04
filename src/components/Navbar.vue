<template>
  <div class="container">
    <nav class="navbar">
      <router-link to="/">Home</router-link>
      <router-link to="/posts">Posts</router-link>
      <router-link to="/store">Store</router-link>
      <router-link to="/composition">Composition</router-link>
      <router-link to="/about">About</router-link>
    </nav>
  </div>
</template>

<script>
export default {

}
</script>

<style scoped>
.navbar {
  display: flex;
  justify-content: flex-end;
}
.navbar a {
  padding: .5rem;
  text-decoration: none;
  color: #333;
}
.navbar a:hover {
  background: #ebebeb;
}
</style>