<template>
  <div class="boards-index">
    <div v-for="board in boards" v-bind:key="board.id">
      {{ board.name }}
      <div>
        <router-link :to="`/boards/${board.id}`">See More</router-link>
      </div>
    </div>
    <form v-on:submit.prevent="createBoard()">
      <h4>Name of new board:</h4>
      <input type="text" v-model="newBoardParams.name" />
      <button>Create!</button>
    </form>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      boards: [],
      name: "",
      posts: {},
      newBoardParams: {},
    };
  },
  created: function () {
    this.indexBoards();
  },
  methods: {
    indexBoards: function () {
      axios
        .get("/boards")
        .then((response) => {
          console.log(response.data);
          this.boards = response.data;
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
    createBoard: function () {
      axios
        .post("/boards", this.newBoardParams)
        .then((response) => {
          console.log(response.data);
          this.boards.push(response.data);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
  },
};
</script>
