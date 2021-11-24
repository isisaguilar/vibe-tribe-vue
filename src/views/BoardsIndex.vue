<template>
  <div class="boards-index">
    <section class="slice sct-color-2 border-bottom">
      <div class="container">
        <div class="section-title section-title--style-1 text-center mb-3">
          <h1 class="section-title-inner heading-1 strong-300 text-normal">
            Boards
          </h1>

          <span class="section-title-delimiter clearfix d-none"></span>
        </div>

        <span class="clearfix"></span>

        <div
          class="
            fluid-paragraph fluid-paragraph-sm
            c-gray-light
            strong-300
            text-center
          "
        >
          <blockquote class="blockquote blockquote--style-1 mt-4 mb-4">
            Its never a good time!!" -Dani
          </blockquote>
        </div>
      </div>
    </section>

    <section class="slice sct-color-1">
      <div class="container">
        <form class="form-default" v-on:submit.prevent="createBoard()">
          <div class="form-group">
            <h2>Create a new board</h2>
            <input
              type="text"
              class="form-control form-control-lg"
              v-model="newBoardParams.name"
              placeholder="Name that board..."
            />

            <button class="btn btn-styled btn-base-1 btn-circle px-4 mt-4">
              Create!
            </button>
          </div>
        </form>
        <div class="row cols-md-space cols-sm-space cols-xs-space">
          <div
            v-for="board in boards"
            v-bind:key="board.id"
            class="col-lg-6 mb-5"
          >
            <div class="px-4">
              <div class="block block-image-holder">
                <div class="pt-5">
                  <h3 class="heading heading-4 strong-600">
                    <router-link :to="`/boards/${board.id}`">{{
                      board.name
                    }}</router-link>
                  </h3>
                </div>
                <div class="block-image has-solid-shadow-right">
                  <router-link :to="`/boards/${board.id}`"
                    >See More
                  </router-link>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
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
      posts: {
        image_url: "",
      },
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
