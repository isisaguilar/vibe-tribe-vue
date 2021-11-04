<template>
  <div class="post-show">
    <h2>{{ post.blurb }}</h2>
    <p>{{ post.blog }}</p>
    <span v-if="post.image_url"><img :src="post.image_url" alt="" /></span>

    <p>{{ post.user }}</p>
    <div v-if="$parent.getUserId() == post.user_id">
      <router-link :to="`/posts/${post.id}/edit`">Edit</router-link>
    </div>
    <form v-on:submit.prevent="createNewComment()">
      <div>
        <label>Body</label> <br />
        <input type="text" v-model="newCommentParams.body" />
      </div>
      <div>
        <label>Image</label> <br />
        <input type="text" v-model="newCommentParams.image_url" />
      </div>
      <button>Post Comment!</button>
    </form>
    <p>{{ newCommentParams }}</p>
    <h3>Comments</h3>
    <div v-for="comment in post.comments" v-bind:key="comment.id">
      <div v-if="$parent.getUserId() == comment.user.id">
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
          <button v-on:click="updateComment(comment)">Edit</button>
          <button v-on:click="destroyComment(comment)">Delete Comment</button>
        </form>
      </div>
      User: {{ comment.user.name }} <br />
      <img v-bind:src="comment.user.image_url" v-bind:alt="comment.user" />
      Comment: {{ comment.body }} {{ comment.image_url }}
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      post: [],
      comment: [],
      user: {},
      newCommentParams: {},
      updateCommentParams: {},
      errors: [],
    };
  },
  created: function () {
    axios.get(`/posts/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.post = response.data;
    });
  },
  methods: {
    createNewComment: function () {
      this.newCommentParams.post_id = this.$route.params.id;
      axios
        .post("/comments", this.newCommentParams)
        .then((response) => {
          console.log(response.data);
          this.comments.push(response.data);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
    updateComment: function () {
      this.updateCommentParams.post_id = this.$route.params.id;
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
  destroyComment: function () {
    this.comment.id = this.$route.params.id;
    axios.delete(`/comments/${this.post.comments.id}`).then((response) => {
      console.log(response.data);
    });
  },
};
</script>
