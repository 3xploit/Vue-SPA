<template>
  <form @submit.prevent>
    <h5>Post create</h5><br>
    <input type="text" class="input" placeholder="Title" v-model="post.title" v-focus>
    <input type="text" class="input" placeholder="Description" v-model="post.body">
    <button class="btn" @click="createPost">Create</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      post: {
        title: '',
        body: ''
      }
    }
  },
  methods: {
    createPost() {
      this.post.id = Date.now()
      this.$emit('create', this.post)
      this.post = {
        title: '',
        body: ''
      }
    }
  }
}
</script>

<style scoped>
.input {
  padding: .75rem;
  width: 100%;
  border: 1px solid #f0f0f0;
}
form {
  display: flex;
  flex-direction: column;
  margin-bottom: 1rem;
}
form .input {
  margin-bottom: .5rem;
}
</style>