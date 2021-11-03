<template>
  <div class="users-show">
    <h1>{{ message }}</h1>
    <h2>
      {{ current_user.name }} <br />
      <img :src="current_user.image_url" alt="" />
    </h2>
    <form action="/users/me">
      <h1>User Info</h1>
      <div>Name: <input type="text" v-model="current_user.name" /></div>

      <br />
      <div>Email: <input type="text" v-model="current_user.email" /></div>

      <br />
      <div>
        Profile Image: <input type="text" v-model="current_user.image_url" />
      </div>
      <button v-on:click="updateUser(current_user)">Update</button>
      <p>{{ updateUserParams }}</p>
      <br />
    </form>
    <div v-for="post in current_user.posts" v-bind:key="post.id">
      <span v-if="post.blurb"
        >Blurb: <br />
        {{ post.blurb }}</span
      >
      <br />
      <span v-if="post.blog"
        >Blog: <br />
        {{ post.blog }}</span
      >
      <br />
      <span v-if="post.image_url">
        Image: <br />
        <img :src="post.image_url" alt=""
      /></span>
      <br />
      <span v-if="post.video_url">
        Video: <br />
        <img :src="post.video_url" alt=""
      /></span>
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
  methods: {
    updateUser: function (user) {
      var updateUserParams = user;
      axios
        .patch("/users/me", updateUserParams)
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
  },
};
</script>
