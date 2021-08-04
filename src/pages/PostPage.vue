<template>
  <div>
    <h1>Posts</h1>
    <div class="app__btns">
      <button class="btn" @click="showPopup">Create</button>
      <input type="text" v-model="searchQuery" placeholder="Search" v-focus>
      <my-select
          v-model="selectedSort"
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

export default {
  components: {
    MySelect,
    MyPopup,
    PostList,
    PostForm
  },
  data() {
    return {
      posts: [],
      popupVisible: false,
      isLoading: false,
      selectedSort: '',
      searchQuery: '',
      page: 1,
      limit: 10,
      totalPages: 0,
      sortOptions: [
        {value: 'title', name: 'By title'},
        {value: 'body', name: 'By body'},
      ],
    }
  },
  methods: {
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
    // changePage(pageNumber) {
    //   this.page = pageNumber;
    //   // this.fetchPosts()
    // },
    async fetchPosts() {
      try {
        this.isLoading = true;
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit);
        this.posts = response.data;
      } catch (e) {
        alert('Error')
      } finally {
        this.isLoading = false;
      }
    },
    async loadMorePosts() {
      try {
        this.page += 1;
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit);
        this.posts = [...this.posts, ...response.data];
      } catch (e) {
        alert('Error')
      }
    },
  },
  mounted() {
    this.fetchPosts();
    // const options = {
    //   rootMargin: '0px',
    //   threshold: 1.0
    // }
    // const callback = (entries, observer) => {
    //   if (entries[0].isIntersecting && this.page < this.totalPages) {
    //     this.loadMorePosts();
    //   }
    // };
    // const observer = new IntersectionObserver(callback, options);
    // observer.observe(this.$refs.observer);
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    }
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