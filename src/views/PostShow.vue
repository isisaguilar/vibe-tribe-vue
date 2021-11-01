<template>
  <div class="post-show">
    <h2>Post</h2>
    <img src="" alt="" />
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
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
