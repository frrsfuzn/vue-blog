<template>
  <div class="tag">
    <h1>Tag: {{ tag }}</h1>
    <div v-if="error">{{ error }}</div>
    <div v-if="filteredPosts.length">
      <PostList :posts="filteredPosts" />
    </div>
    <div v-else><Spinner /></div>
  </div>
</template>

<script>
import { computed } from "@vue/runtime-core";
import getPosts from "../composable/getPosts";
import Spinner from "../components/Spinner.vue";
import PostList from "../components/PostList.vue";
export default {
  props: ["tag"],
  components: { Spinner, PostList },
  setup(props) {
    const { posts, error, load } = getPosts();
    const filteredPosts = computed(() =>
      posts.value.filter((post) => post.tags.includes(props.tag))
    );
    load();
    return { filteredPosts, error };
  },
};
</script>

<style>
</style>