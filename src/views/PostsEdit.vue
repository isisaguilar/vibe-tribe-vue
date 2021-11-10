<template>
  <div class="posts-edit">
    <form v-on:submit.prevent="updatePost()">
      <h1>Update Post</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Blurb:</label>
        <input type="text" v-model="updatePostParams.blurb" />
      </div>
      <div>
        <label>Blog:</label>
        <input type="text" v-model="updatePostParams.blog" />
      </div>
      <div>
        <label>Image:</label>
        <input type="text" v-model="updatePostParams.image_url" />
      </div>
      <div>
        <label>Video:</label>
        <input type="text" v-model="updatePostParams.video_url" />
      </div>
      <input type="submit" value="Update" />
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      updatePostParams: {},
      errors: [],
    };
  },
  created: function () {
    axios.get(`/posts/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.updatePostParams = response.data;
    });
  },

  methods: {
    updatePost: function () {
      axios
        .patch(`/posts/${this.updatePostParams.id}`, this.updatePostParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push(`/posts/${response.data.id}`);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
