<template>
  <div class="posts-edit">
    <section
      class="slice-lg has-bg-cover bg-size-cover"
      style="
        background-image: url(https://images.unsplash.com/photo-1454817481404-7e84c1b73b4a?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MzN8fHVuaXZlcnNlfGVufDB8fDB8fA%3D%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=60);
        background-position: bottom center;
      "
    >
      <div class="container">
        <div class="row justify-content-center cols-xs-space">
          <div class="col-lg-6">
            <div class="form-card form-card--style-2 z-depth-2-top">
              <div class="form-header text-center">
                <div class="form-header-icon">
                  <i class="icon ion-log-in"></i>
                </div>
              </div>
              <div class="form-body">
                <div class="text-center px-2">
                  <h4 class="heading heading-4 strong-400 mb-4">Edit Post</h4>
                </div>

                <form
                  class="form-default mt-4"
                  v-on:submit.prevent="updatePost()"
                >
                  <ul>
                    <li v-for="error in errors" v-bind:key="error">
                      {{ error }}
                    </li>
                  </ul>
                  <div class="row">
                    <div class="col-md-12">
                      <div class="form-group">
                        <label class="control-label">Blurb</label>
                        <input
                          type="text"
                          class="form-control form-control-lg"
                          v-model="updatePostParams.blurb"
                        />
                      </div>
                    </div>
                  </div>

                  <div class="row">
                    <div class="col-md-12">
                      <div class="form-group">
                        <label class="control-label">Blog</label>
                        <textarea
                          class="form-control form-control-lg textarea-autogrow"
                          type="text"
                          v-model="updatePostParams.blog"
                        ></textarea>
                      </div>
                    </div>
                  </div>

                  <div class="row">
                    <div class="col-md-12">
                      <div class="form-group">
                        <label class="control-label">Image Url</label>
                        <input
                          type="text"
                          class="form-control form-control-lg"
                          v-model="updatePostParams.image_url"
                        />
                      </div>
                    </div>
                  </div>

                  <div class="row">
                    <div class="col-md-12">
                      <div class="form-group">
                        <label class="control-label">Video Url</label>
                        <input
                          type="text"
                          class="form-control form-control-lg"
                          v-model="updatePostParams.video_url"
                        />
                      </div>
                    </div>
                  </div>

                  <button type="submit" class="btn btn-styled btn-base-1 btn-circle px-4 mt-5">
                    Post it
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
