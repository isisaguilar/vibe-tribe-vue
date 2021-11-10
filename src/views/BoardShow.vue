<template>
  <div class="board-show">
    <h1>{{ board.name }}</h1>
    <div v-for="post_board in board.post_boards" v-bind:key="post_board.id">
      <br />

      <p>{{ post_board.post.blog }}</p>
      <br />
      <p>{{ post_board.post.blurb }}</p>
      <img
        v-bind:src="post_board.post.image_url"
        v-bind:alt="post_board.post.image_url"
      />
      <br />
      <button v-on:click="unsavePost(post_board)">Unsave Post</button>
    </div>
    <form v-on:submit.prevent="updateBoard()">
      <h1>Update Board</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Name:</label>
        <input type="text" v-model="board.name" />
      </div>
      <input type="submit" value="Update" />
    </form>

    <button v-on:click="destroyBoard()">Delete Board</button>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      errors: [],
      board: {},
    };
  },
  created: function () {
    axios.get(`/boards/${this.$route.params.id}`).then((response) => {
      console.log("Board object", response.data);
      this.board = response.data;
    });
  },
  methods: {
    updateBoard: function () {
      axios
        .patch(`/boards/${this.board.id}`, this.board)
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    destroyBoard: function () {
      axios.delete(`/boards/${this.board.id}`).then((response) => {
        console.log(response.data);
        this.$router.push("/boards");
      });
    },
    unsavePost: function (post_board) {
      axios.delete(`/post_boards/${post_board.id}`).then((response) => {
        console.log(response.data);
        let index = this.board.post_boards.indexOf(post_board);
        this.board.post_boards.splice(index, 1);
      });
    },
  },
};
</script>
// 256 0354
