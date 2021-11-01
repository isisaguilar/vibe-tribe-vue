<template>
  <div class="users-show">
    <h1>{{ message }}</h1>
    <h2>{{ current_user.name }}</h2>
    <img src="" alt="" />
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
      Blurb: {{ post.blurb }} <br />
      Blog: {{ post.blog }} <br />
      Image: {{ post.image_url }} <br />
      Video: {{ post.video_url }}
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
