<template>
  <div class="post-show">
    <h2>{{ post.blurb }}</h2>
    <p>{{ post.blog }}</p>
    <span v-if="post.image_url"
      ><img v-bind:src="post.image_url" v-bind:alt="post.image_url"
    /></span>
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
        <form v-on:submit.prevent="updateComment(comment)">
          <h1>Update Comment</h1>
          <ul>
            <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
          </ul>
          <div>
            <label>Body:</label>
            <input type="text" v-model="comment.body" />
          </div>
          <div>
            <label>Image:</label>
            <input type="text" v-model="comment.image_url" />
          </div>
          {{ comment.id }}

          {{ comment.user.id }}
          <input type="submit" class="btn btn-primary" value="Submit" />
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
      newCommentParams: {},
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
          this.post.comments.push(response.data);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },

    updateComment: function (comment) {
      console.log(comment);
      axios
        .patch(`/comments/${comment.id}`, comment)
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    destroyComment: function (comment) {
      axios.delete("/comments/" + comment.id).then((response) => {
        console.log(response.data);
        let index = this.post.comments.indexOf(comment);
        this.post.comments.splice(index, 1);
      });
    },
    getUserId: function () {
      return localStorage.user_id;
    },
  },
};
</script>
