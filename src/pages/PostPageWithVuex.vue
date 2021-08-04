<template>
  <div>
    <h1>Posts</h1>
    <div class="app__btns">
      <button class="btn" @click="showPopup">Create</button>
      <my-input
          :model-value="searchQuery"
          @update:model-value="setSearchQuery"
          placeholder="Search"
          v-focus
      />
      <my-select
          :model-value="selectedSort"
          @update:model-value="setSelectedSort"
          :options="sortOptions"
      />
    </div>
    <my-popup v-model:show="popupVisible">
      <post-form @create="createPost" />
    </my-popup>
    <post-list
        :posts="sortedAndSearchedPosts"
        @remove="removePost"
        v-if="!isLoading"
    />
    <p v-else>Loading posts ...</p>
    <div v-intersection="loadMorePosts" class="observer"></div>
    <!--    <div class="page__wrapper">-->
    <!--      <span-->
    <!--          v-for="pageNumber in totalPages"-->
    <!--          :key="pageNumber"-->
    <!--          :class="{'current': page === pageNumber}"-->
    <!--          @click="changePage(pageNumber)"-->
    <!--      >-->
    <!--        {{ pageNumber }}-->
    <!--      </span>-->
    <!--    </div>-->
  </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import MyPopup from "@/components/ui/MyPopup";
import axios from 'axios'
import MySelect from "@/components/ui/MySelect";
import {mapState, mapGetters, mapActions, mapMutations} from 'vuex';

export default {
  components: {
    MySelect,
    MyPopup,
    PostList,
    PostForm
  },
  data() {
    return {
      popupVisible: false,
    }
  },
  methods: {
    ...mapMutations({
      setPage: 'post/setPage',
      setSearchQuery: 'post/setSearchQuery',
      setSelectedSort: 'post/setSelectedSort'
    }),
    ...mapActions({
      loadMorePosts: 'post/loadMorePosts',
      fetchPosts: 'post/fetchPosts'
    }),
    createPost(post) {
      this.posts.push(post);
      this.popupVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id);
    },
    showPopup() {
      this.popupVisible = true;
    },
  },
  mounted() {
    this.fetchPosts();
  },
  computed: {
    ...mapState({
      posts: state => state.post.posts,
      isLoading: state => state.post.isLoading,
      selectedSort: state => state.post.selectedSort,
      searchQuery: state => state.post.searchQuery,
      page: state => state.post.page,
      limit: state => state.post.limit,
      totalPages: state => state.post.totalPages,
      sortOptions: state => state.post.sortOptions
    }),
    ...mapGetters({
      sortedPosts: 'post/sortedPosts',
      sortedAndSearchedPosts: 'post/sortedAndSearchedPosts'
    })
  },
  watch: {
    // page() {
    //   this.fetchPosts()
    // }
  }
}
</script>

<style scoped>
.app__btns {
  display: flex;
  justify-content: space-between;
  margin: 1rem 0;
}
input[type="text"], select {
  border: 1px solid #ddd;
  padding: .6rem 1rem;
  border-radius: .25rem;
}
.page__wrapper {
  display: flex;
}
.page__wrapper span {
  padding: .5rem;
  border: 1px solid #ddd;
  cursor: pointer;
}
.page__wrapper span:not(:last-child) {
  border-right: none;
}
.page__wrapper .current {
  color: #fff;
  background: #1b41b3;
}
</style>