<template>
  <div class="post">
    <div>
      <div><strong>{{ post.title }}</strong></div>
      <div>{{ post.body }}</div>
    </div>
    <div class="post__btns">
      <button class="btn" @click="$router.push(`/posts/${post.id}`)">View</button>
      <button class="btn" @click="$emit('remove', post)">Delete</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    post: {
      type: Object,
      required: true,
    }
  }
}
</script>

<style scoped>
.post {
  padding: 1rem;
  background: #f0f0f0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-radius: .25rem;
}
.post:not(:last-child) {
  margin-bottom: .5rem;
}
.post__btns {
  display: flex;
}
</style>