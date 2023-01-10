<template>
  <div class="posts">
    <div class="posts__post" v-for="post in paginatedPosts" :key="post.id">
      <div class="posts__post-title">{{ post.id }} {{ post.title }}</div>
      <div class="posts__post-body">{{ post.body }}</div>
    </div>

    <div class="posts__pagination">
      <vue-awesome-paginate
        :total-items="totalItems"
        :items-per-page="itemsPerPage"
        :max-pages-shown="maxPagesShown"
        v-model="currentPage"
        :on-click="onClickHandler"
      />
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import getPosts from "./composables/getPosts";

export default {
  setup() {
    const { posts, load } = getPosts();
    const totalItems = computed(() => posts.value.length);
    const itemsPerPage = ref(10);
    const maxPagesShown = ref(5); // amount of max pagination links
    const currentPage = ref(1);
    // starter index for paginatedPosts
    const fromIdx = computed(
      () => (currentPage.value - 1) * itemsPerPage.value
    );

    load();
    // Paginate event page. Is equal to pagination item number (it's >= 1)
    const onClickHandler = (page) => {
      currentPage.value = page;
    };

    const paginatedPosts = computed(() =>
      [...posts.value].splice(fromIdx.value, itemsPerPage.value)
    );

    return {
      posts,
      totalItems,
      itemsPerPage,
      maxPagesShown,
      currentPage,
      fromIdx,
      onClickHandler,
      paginatedPosts,
    };
  },
};
</script>

<style>
.posts {
  padding: 20px;
}
.posts__post {
  padding: 10px;
  background: rgb(211, 210, 210);
  border-radius: 8px;
  margin-bottom: 20px;
}
.posts__post:last-child {
  margin-bottom: 0;
}
.posts__post-title {
  font-size: 18px;
  font-weight: bold;
  padding-bottom: 10px;
}
.posts__post-body {
  font-size: 16px;
}
.posts__pagination {
  display: flex;
  justify-content: center;
}

.pagination-container {
  display: flex;
  column-gap: 10px;
}
.paginate-buttons {
  height: 40px;
  width: 40px;
  border-radius: 20px;
  cursor: pointer;
  background-color: rgb(242, 242, 242);
  border: 1px solid rgb(217, 217, 217);
  color: black;
}
.paginate-buttons:hover {
  background-color: #d8d8d8;
}
.active-page {
  background-color: #3498db;
  border: 1px solid #3498db;
  color: white;
}
.active-page:hover {
  background-color: #2988c8;
}
</style>
