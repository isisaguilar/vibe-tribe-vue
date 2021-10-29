<template>
  <div class="users-show">
    <h1>{{ message }}</h1>
    <h2>{{ current_user.name }}</h2>
    <form action="/users/me">
      <h1>User Info</h1>
      <p>Name: <input type="text" v-model="current_user.name" /></p>
      <br />
      <p>Email: <input type="text" v-model="current_user.email" /></p>
      <br />
      <p>
        Profile Image: <input type="text" v-model="current_user.image_url" />
      </p>
    </form>
    <div v-for="post in current_user.posts" v-bind:key="post.id">
      {{ post.blurb }} <br />
      {{ post.blog }} <br />
      {{ post.image_url }} <br />
      {{ post.video_url }}
    </div>
    <!-- <h3>{{ current_user }}</h3> -->
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to User show page!",
      current_user: {},
    };
  },
  created: function () {
    axios.get("/users/me").then((response) => {
      console.log(response.data);
      this.current_user = response.data;
    });
  },
  methods: {},
};
</script>
