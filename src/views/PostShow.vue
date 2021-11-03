<template>
  <div class="post-show">
    <h2>{{ post.blurb }}</h2>
    <p>{{ post.blog }}</p>
    <span v-if="post.image_url"><img :src="post.image_url" alt="" /></span>

    <p>{{ post.user }}</p>
    <div v-if="$parent.getUserId() == post.user_id">
      <router-link :to="`/posts/${post.id}/edit`">Edit</router-link>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      post: {},
    };
  },
  created: function () {
    axios.get(`/posts/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.post = response.data;
    });
  },
  methods: {},
};
</script>
