<template>
  <div class="posts-index">
    <section class="slice sct-color-2 border-bottom">
      <div class="container">
        <div class="section-title section-title--style-1 text-center mb-3">
          <h3 class="section-title-inner heading-1 strong-300 text-normal">
            Posts
            <router-link
              to="/posts/new"
              class="btn btn-sm btn-styled btn-base-1"
            >
              New Post
            </router-link>
          </h3>

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
          Isis comes up with a cool thing to put here
        </div>
      </div>
    </section>

    <section class="slice sct-color-1">
      <div class="container">
        <div class="row cols-md-space cols-sm-space cols-xs-space">
          <div v-for="post in posts" v-bind:key="post.id" class="col-lg-6 mb-5">
            <div class="px-4">
              <div class="block block-image-holder">
                <div class="block-image has-solid-shadow-right">
                  <router-link :to="`/posts/${post.id}`">
                    <img
                      v-if="post.image_url"
                      :src="post.image_url"
                      class="img-fluid img-center"
                    />
                    <img
                      v-if="!post.image_url && post.video_url"
                      :src="post.video_url"
                      class="img-fluid img-center"
                    />
                  </router-link>
                </div>

                <div class="pt-5">
                  <h3 class="heading heading-4 strong-600">
                    <router-link :to="`/posts/${post.id}`">{{
                      post.blurb
                    }}</router-link>
                  </h3>
                </div>

                <div class="block-author">
                  <div class="author-info">
                    <div class="author-name">
                      <span>By {{ post.user.name }}</span>
                    </div>
                  </div>
                </div>

                <div class="row align-items-center">
                  <div class="col-10">
                    <select
                      v-model="newPostBoardParams.board_id"
                      class="form-control selectpicker"
                      data-minimum-results-for-search="Infinity"
                    >
                      <option disabled value="">Please select board</option>
                      <option
                        v-for="board in user.boards"
                        :key="board.id"
                        :value="board.id"
                      >
                        {{ board.name }}
                      </option>
                    </select>
                  </div>

                  <div class="col-2">
                    <button
                      class="btn btn-sm btn-styled btn-base-1"
                      v-on:click="createPostBoard(post.id)"
                    >
                      Save
                    </button>
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

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      posts: [],
      user: {},

      newPostBoardParams: {},
      name: "",
      boards: "",
      options: [],
    };
  },
  created: function () {
    this.indexPosts();
    axios.get("/users/me").then((response) => {
      console.log(response.data);
      this.user = response.data;
    });
  },
  methods: {
    indexPosts: function () {
      axios
        .get("/posts")
        .then((response) => {
          console.log(response.data);
          this.posts = response.data;
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },

    createPostBoard: function (post) {
      this.newPostBoardParams.post_id = post;
      axios
        .post("/post_boards", this.newPostBoardParams)
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
