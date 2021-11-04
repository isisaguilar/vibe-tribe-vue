<template>
  <div class="posts-index">
    <h1>Posts</h1>
    <h2>Create New Post</h2>
    <form v-on:submit.prevent="createNewPost()">
      <div>
        <label>Blog</label>
        <input type="text" v-model="newPostParams.blog" />
      </div>
      <div>
        <label>Blurb</label>
        <input type="text" v-model="newPostParams.blurb" />
      </div>
      <div>
        <label>Image</label>
        <input type="text" v-model="newPostParams.image_url" />
      </div>
      <div>
        <label>Video</label>
        <input type="text" v-model="newPostParams.video_url" />
      </div>
      <button>Create!</button>
    </form>
    <p>{{ newPostParams }}</p>
    <div v-for="post in posts" v-bind:key="post.id">
      <span v-if="post.blurb">Blurb: {{ post.blurb }}</span> <br />
      <span v-if="post.blog">Blog: {{ post.blog }}</span> <br />
      <span v-if="post.image_url">
        Image: <img :src="post.image_url" alt=""
      /></span>
      <br />
      <span v-if="post.video_url">
        Video: <img :src="post.video_url" alt=""
      /></span>
      <br />

      <router-link :to="`/posts/${post.id}`">See Post Details</router-link>
    </div>
  </div>
</template>

<style>
img {
  width: 200px;
  height: 300px;
  object-fit: cover;
}
</style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      posts: [],
      user: {},
      newPostParams: {},
      name: "",
    };
  },
  created: function () {
    this.indexPosts();
  },
  methods: {
    indexPosts: function () {
      axios
        .get("/posts")
        .then((response) => {
          console.log(response.data);
          this.posts = response.data;
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
    createNewPost: function () {
      axios
        .post("/posts", this.newPostParams)
        .then((response) => {
          console.log(response.data);
          this.posts.push(response.data);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
  },
};
</script>
