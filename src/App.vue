<template>
  <h1>Страница с постами</h1>
  <div class="app">
    <my-input 
      v-model="searchQuery"
      placeholder="Поиск..."/>
    <div class="app_btns">
      <my-button 
        @click="showDialog">
        Создать пост
      </my-button>
      <my-select 
        v-model="selectedSort"
        :options="sortOptions"/>
    </div>
    <my-dialog 
      v-model:show="dialogVisible">
      <post-form
        @create="createPost"/>
    </my-dialog>
    <post-list 
    :posts="searchAndSortedPost"
    @remove="removePost"
    v-if="!isPostsLoading"/>
    <div v-else>Идёт загрузка</div>
  </div>
</template>

<script>
import PostForm from "./components/PostForm.vue";
import PostList from "./components/PostList.vue";
import axios from "axios";
import MySelect from './components/UI/MySelect.vue';

export default {
  components: {
    PostForm, PostList,
    MySelect,
  },

  data() {
    return {
      posts: [
      ],

      dialogVisible: false,
      isPostsLoading: false,
      selectedSort: 'standart',
      searchQuery: '',
      sortOptions: [
        {value: 'standart', name: 'Выбери для сортировки', selected: true},
        {value: 'title', name: 'По названию', selected: false},
        {value: 'body', name: 'По содежимому', selected: false}
      ] 
    }
  },
  
  methods: {
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
    },

    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },

    showDialog() {
      this.dialogVisible = true;
    },

    async fetchPosts() {
      try {
        this.isPostsLoading = true
        const response = await axios.get("https://jsonplaceholder.typicode.com/posts?_limit=10");
        this.posts = response.data;
        console.log(response);
      } catch (e) {
        alert("Ошибка")
      } finally {
        this.isPostsLoading = false
      }
    }
  },

  mounted() {
    this.fetchPosts();
  },

  computed: {
    sortedPost() {
      if(this.selectedSort === 'standart') {
        return [...this.posts]
      } else {
        return [...this.posts].sort( (post1, post2) => post1[this.selectedSort].localeCompare(post2[this.selectedSort]))
      }      
    },

    searchAndSortedPost() {
      return this.sortedPost.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    }
  }
}
</script>

<style>
* {
  margin: 0;
  box-sizing: border-box;
  padding: 0;
}

.app {
  padding: 20px
}

.app_btns {
  display: flex;
  justify-content: space-between;
  margin: 20px 0;
}
</style>
