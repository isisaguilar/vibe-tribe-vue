<template>
  <div class="post-show">
    <section class="slice-sm sct-color-1">
      <div class="container container-xs">
        <div class="block block-post">
          <div class="block-body block-post-body">
            <div>
              <h3 class="heading heading-4 strong-400 text-normal">
                Posted by: {{ post.user.name }}
              </h3>
              <router-link
                v-if="$parent.getUserId() == post.user_id"
                :to="`/posts/${post.id}/edit`"
                >Edit</router-link
              >

              <blockquote class="blockquote blockquote--style-1 mt-4 mb-4">
                {{ post.blurb }}
              </blockquote>

              <p>
                {{ post.blog }}
              </p>

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
            </div>

            <div class="block-post-comment-action mt-5">
              <div class="row align-items-center">
                <div class="col col-md-6 col-12">
                  <h5 class="heading heading-5 strong-500">Comments</h5>
                </div>

                <div class="col col-md-6 col-12 text-right">
                  <a
                    href="#form_comment"
                    class="heading heading-sm strong-500 scroll-me"
                  >
                    Add your comment
                  </a>
                </div>
              </div>
            </div>

            <ul class="block-post-comments block-post-comments--style-1">
              <li>
                <!-- Comment 1 -->
                <div
                  v-for="comment in post.comments"
                  v-bind:key="comment.id"
                  class="block block-comment"
                >
                  <div class="block-image">
                    <img
                      v-bind:src="comment.user.image_url"
                      v-bind:alt="comment.user"
                      class="img-circle"
                    />
                  </div>
                  <div class="block-body">
                    <div class="block-body-inner">
                      <h3 class="heading heading-6">
                        {{ comment.user.name }}
                      </h3>

                      <div class="comment-text">
                        <form
                          v-if="currentComment === comment"
                          v-on:submit.prevent="updateComment(comment)"
                          class="form-default"
                        >
                          <div class="row">
                            <div class="col-sm-12">
                              <div class="form-group">
                                <textarea
                                  rows="3"
                                  v-model="comment.body"
                                  class="form-control"
                                ></textarea>
                              </div>
                            </div>
                          </div>
                          <div class="row">
                            <div class="col-sm-12">
                              <div class="form-group">
                                <label for="" class="text-uppercase sr-only"
                                  >Image</label
                                ><input
                                  type="text"
                                  v-model="comment.image_url"
                                  class="form-control"
                                  placeholder="Image Url"
                                />
                              </div>
                            </div>
                          </div>
                          <button
                            type="submit"
                            class="btn btn-styled btn-base-4 mt-1"
                          >
                            Update comment
                          </button>
                          <button
                            v-on:click="destroyComment(comment)"
                            type="submit"
                            class="btn btn-styled btn-base-4 mt-1 ml-2"
                          >
                            Delete comment
                          </button>
                        </form>
                        <div v-else>
                          {{ comment.body }}
                          <img
                            v-bind:src="comment.image_url"
                            v-bind:alt="comment.image_url"
                            class="img-fluid img-center"
                          />
                        </div>
                      </div>
                      <div
                        v-if="$parent.getUserId() == comment.user.id"
                        class="comment-options"
                      >
                        <a
                          v-if="currentComment !== comment"
                          v-on:click="currentComment = comment"
                          >Edit Comment</a
                        >
                      </div>
                    </div>
                  </div>
                </div>
              </li>
            </ul>

            <!-- Comment form -->
            <div
              id="form_comment"
              class="feature feature--boxed-border feature--bg-3 mt-5"
            >
              <div class="px-3">
                <div class="text-center">
                  <h5 class="heading heading-3 strong-400 mb-4">
                    Add your comment
                  </h5>
                  <p class="">Be kind. Spread love.</p>
                </div>

                <form
                  class="form-default mt-5"
                  v-on:submit.prevent="createNewComment()"
                  role="form"
                >
                  <div class="row">
                    <div class="col-sm-12">
                      <div class="form-group">
                        <label for="" class="text-uppercase sr-only"
                          >Message</label
                        >
                        <textarea
                          v-model="newCommentParams.body"
                          class="form-control"
                          rows="5"
                          placeholder="Light&Love"
                        ></textarea>
                      </div>
                    </div>
                  </div>

                  <div class="row">
                    <div class="col-sm-12">
                      <div class="form-group">
                        <label for="" class="text-uppercase sr-only"
                          >Image</label
                        >
                        <input
                          v-model="newCommentParams.image_url"
                          type="text"
                          class="form-control input-lg"
                          placeholder="Image"
                        />
                      </div>
                    </div>
                  </div>

                  <button type="submit" class="btn btn-styled btn-base-4 mt-1">
                    Post comment
                  </button>
                </form>
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
      post: [],
      newCommentParams: {},
      errors: [],
      currentComment: {},
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
          this.currentComment = {};
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
