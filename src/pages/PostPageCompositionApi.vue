<template>
  <div>
    <h1>Posts</h1>
    <div class="app__btns">
<!--      <button class="btn" @click="showPopup">Create</button>-->
      <input type="text" v-model="searchQuery" placeholder="Search" v-focus>
      <my-select
          v-model="selectedSort"
          :options="sortOptions"
      />
    </div>
    <my-popup v-model:show="popupVisible">
      <post-form
      />
    </my-popup>
    <post-list
        :posts="sortedAndSearchedPosts"
        v-if="!isLoading"
    />
    <p v-else>Loading posts ...</p>
<!--    <div v-intersection="loadMorePosts" class="observer"></div>-->
  </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import MyPopup from "@/components/ui/MyPopup";
import axios from 'axios'
import MySelect from "@/components/ui/MySelect";
import {usePosts} from "@/hooks/usePosts";
import useSortedPosts from "@/hooks/useSortedPosts";
import useSortedAndSearchedPosts from "@/hooks/useSortedAndSearchedPosts";

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
      sortOptions: [
        {value: 'title', name: 'By title'},
        {value: 'body', name: 'By body'},
      ],
    }
  },
  setup(props) {
    const {posts, totalPages, isLoading} = usePosts(10);
    const {selectedSort, sortedPosts} = useSortedPosts(posts);
    const {searchQuery, sortedAndSearchedPosts} = useSortedAndSearchedPosts(sortedPosts);

    return {
      posts,
      totalPages,
      isLoading,
      selectedSort,
      sortedPosts,
      searchQuery,
      sortedAndSearchedPosts
    }
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