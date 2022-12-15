<template>
  <div class="tag">
    <h1>Tag: {{ tag }}</h1>
    <div v-if="error">{{ error }}</div>
    <div v-if="filteredPosts.length" class="layout">
      <PostList :posts="filteredPosts" />
      <TagCloud :posts="posts" />
    </div>
    <div v-else><Spinner /></div>
  </div>
</template>

<script>
import { computed } from "@vue/runtime-core";
import getPosts from "../composable/getPosts";
import Spinner from "../components/Spinner.vue";
import PostList from "../components/PostList.vue";
import TagCloud from "../components/TagCloud.vue";
export default {
  props: ["tag"],
  components: { Spinner, PostList, TagCloud },
  setup(props) {
    const { posts, error, load } = getPosts();
    const filteredPosts = computed(() =>
      posts.value.filter((post) => post.tags.includes(props.tag))
    );
    load();
    return { posts, filteredPosts, error };
  },
};
</script>

<style>
.tag {
  max-width: 1200px;
  margin: 0 auto;
  padding: 10px;
}
.layout {
  display: grid;
  grid-template-columns: 3fr 1fr;
  gap: 20px;
}
</style>