<template>
  <div class="comments-edit">
    <form v-on:submit.prevent="updateComment()">
      <h1>Update Comment</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Body:</label>
        <input type="text" v-model="updateCommentParams.body" />
      </div>
      <div>
        <label>Image:</label>
        <input type="text" v-model="updateCommentParams.image_url" />
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
      comments: {},
      updateCommentsParams: {},
      errors: [],
    };
  },
  created: function () {
    axios.get(`/comments/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.updatePostParams = response.data;
    });
  },

  methods: {
    updateComment: function () {
      axios
        .patch(`/comments/${this.updateCommentParams.id}`)
        .then((response) => {
          console.log(response.data);
          this.$router.push(`/comments/${response.data.id}`);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
