<template>
  <div class="board-show">
    <section class="slice sct-color-2 border-bottom">
      <div class="container">
        <div class="section-title section-title--style-1 text-center mb-3">
          <h1 class="section-title-inner heading-1 strong-300 text-normal">
            <router-link
              to="/posts/new"
              class="btn btn-styled btn-base-1 btn-circle px-6 mt-5"
            >
              New Post
            </router-link>
          </h1>

          <span class="section-title-delimiter clearfix d-none"></span>
        </div>

        <span class="clearfix"></span>

        <div
          class="
            fluid-paragraph fluid-paragraph-sm
            c-gray-light
            strong-100
            text-center
          "
        >
          <blockquote class="blockquote blockquote--style-1 mt-4 mb-4">
            {{ board.name }}
          </blockquote>
        </div>
      </div>
    </section>

    <section class="slice sct-color-1">
      <div class="container">
        <form class="form-default" v-on:submit.prevent="updateBoard()">
          <div class="form-group">
            <h2>Update board...</h2>
            <input
              type="text"
              class="form-control form-control-lg"
              v-model="board.name"
            />

            <button
              type="submit"
              value="Update"
              class="btn btn-styled btn-base-1 btn-circle px-4 mt-5 ml-2"
            >
              Update
            </button>
            <button
              v-on:click="destroyBoard()"
              class="btn btn-styled btn-base-1 btn-circle px-4 mt-5 ml-2"
            >
              Delete
            </button>
          </div>
        </form>
        <div class="row cols-md-space cols-sm-space cols-xs-space">
          <div class="row align-items-center">
            <div class="col-10"></div>
          </div>
          <div
            v-for="post_board in board.post_boards"
            v-bind:key="post_board.id"
            class="col-lg-6 mb-5"
          >
            <div class="px-4">
              <div class="block block-image-holder">
                <img
                  v-bind:src="post_board.post.image_url"
                  v-bind:alt="post_board.post.image_url"
                  class="img-fluid img-center"
                />

                <div class="pt-5">
                  <router-link :to="`/posts/${post.id}`">
                    <h3 class="heading heading-4 strong-600">
                      {{ post_board.post.blurb }}
                    </h3>
                  </router-link>
                </div>

                <div class="block-author">
                  <div class="author-info">
                    <div class="author-name">
                      <button
                        class="btn btn-styled btn-base-1 btn-circle px-4 mt-5"
                        v-on:click="unsavePost(post_board)"
                      >
                        Unsave Post
                      </button>
                    </div>
                  </div>
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
      errors: [],
      board: {},
      post: "",
    };
  },
  created: function () {
    axios.get(`/boards/${this.$route.params.id}`).then((response) => {
      console.log("Board object", response.data);
      this.board = response.data;
    });
    axios.get(`/posts/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.post = response.data;
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
