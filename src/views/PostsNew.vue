<template>
  <div class="signup">
    <section
      class="slice-lg has-bg-cover bg-size-cover"
      style="
        background-image: url(https://images.unsplash.com/photo-1538370965046-79c0d6907d47?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MjF8fHVuaXZlcnNlfGVufDB8fDB8fA%3D%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=60);
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
                  <h4 class="heading heading-4 strong-400 mb-4">
                    Create A New Post!
                  </h4>
                </div>

                <form
                  class="form-default mt-4"
                  v-on:submit.prevent="createNewPost()"
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
                          v-model="newPostParams.blurb"
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
                          v-model="newPostParams.Blog"
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
                          v-model="newPostParams.image_url"
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
                          v-model="newPostParams.video_url"
                        />
                      </div>
                    </div>
                  </div>

                  <button
                    type="submit"
                    class="btn btn-styled btn-base-1 btn-circle px-4 mt-5"
                  >
                    Create!
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
      newPostParams: {},
      errors: [],
    };
  },
  methods: {
    createNewPost: function () {
      axios
        .post("/posts", this.newPostParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push(`/posts/${response.data.id}`);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
  },
};
</script>
