<template>
  <div class="users-show">
    <section class="slice-sm sct-color-1">
      <div class="profile">
        <div class="container">
          <div class="row cols-xs-space cols-sm-space cols-md-space">
            <div class="col-lg-3">
              <div
                class="
                  sidebar sidebar-inverse
                  bg-base-1
                  sidebar--style-2
                  no-border
                  stickyfill
                "
              >
                <div class="widget">
                  <!-- Profile picture -->
                  <div class="profile-picture profile-picture--style-2">
                    <img
                      :src="currentUser.image_url"
                      alt=""
                      class="img-center"
                    />
                  </div>

                  <!-- Profile details -->
                  <div class="profile-details mb-4">
                    <div v-if="editable" class="form-group">
                      <input
                        type="text"
                        class="form-control form-control-lg"
                        v-model="currentUser.image_url"
                      />
                    </div>
                    <div v-if="editable" class="form-group">
                      <input
                        type="text"
                        class="form-control form-control-lg"
                        v-model="currentUser.name"
                      />
                    </div>
                    <h2
                      v-else
                      class="heading heading-6 strong-600 profile-name"
                    >
                      {{ currentUser.name }}
                    </h2>
                  </div>

                  <hr />

                  <ul class="categories categories--style-3 mt-3">
                    <li>
                      <div v-if="editable" class="form-group">
                        <input
                          type="text"
                          class="form-control form-control-lg"
                          v-model="currentUser.email"
                        />
                      </div>
                      <a v-else href="#">
                        <i class="ion-email"></i>
                        <span class="category-name">
                          {{ currentUser.email }}
                        </span>
                      </a>
                    </li>
                    <li>
                      <a
                        v-if="editable"
                        href="#"
                        class="active"
                        v-on:click="updateUser()"
                        ><i class="ion-refresh"></i>
                        <span class="category-name"> Update </span>
                      </a>
                      <a
                        v-else
                        href="#"
                        class="active"
                        v-on:click="editable = true"
                      >
                        <i class="ion-edit"></i>
                        <span class="category-name"> Edit </span>
                      </a>
                    </li>
                  </ul>
                </div>
              </div>
            </div>

            <div class="col-lg-9">
              <div class="main-content">
                <div class="page-title">
                  <div class="row align-items-center">
                    <div class="col-lg-6 col-12">
                      <h2
                        class="
                          heading heading-5
                          text-capitalize
                          strong-500
                          mb-0
                        "
                      >
                        <a href="#" class="link text-underline--none">
                          <i class="ion-ios-arrow-back"></i> My Posts
                        </a>
                      </h2>
                    </div>
                  </div>
                </div>
                <div class="widget">
                  <div class="card-actions border-bottom">
                    <div class="row align-items-center">
                      <div class="col-3">
                        <router-link
                          to="/posts/new"
                          class="btn btn-blue btn-icon-left"
                        >
                          <i class="icon ion-plus"></i> New Post
                        </router-link>
                      </div>
                    </div>
                  </div>
                </div>
                <!-- Wishlist items -->
                <div
                  class="
                    row
                    shop-default-wrapper shop-cards-wrapper shop-tech-wrapper
                  "
                >
                  <div class="col-lg-12">
                    <div class="row-wrapper">
                      <div class="row cols-md-space">
                        <div
                          v-for="post in currentUser.posts"
                          v-bind:key="post.id"
                          class="col-lg-4 pb-4"
                        >
                          <div class="card card-product">
                            <div class="card-body">
                              <div class="card-image swiper-js-container">
                                <div class="">
                                  <div
                                    class="
                                      swiper-container
                                      swiper-container-horizontal
                                      swiper-container-undefined
                                    "
                                    data-swiper-items="1"
                                    data-swiper-space-between="0"
                                  >
                                    <div
                                      class="swiper-wrapper"
                                      style="
                                        transform: translate3d(0px, 0px, 0px);
                                      "
                                    >
                                      <div
                                        class="swiper-slide swiper-slide-active"
                                        style="width: 160px"
                                      >
                                        <img
                                          v-if="post.image_url"
                                          :src="post.image_url"
                                          class="
                                            img-fluid img-center img-primary
                                          "
                                        />
                                        <img
                                          v-if="post.video_url"
                                          :src="post.video_url"
                                          class="
                                            img-fluid img-center img-primary
                                          "
                                        />
                                      </div>
                                    </div>
                                  </div>
                                </div>
                              </div>

                              <h2
                                class="heading heading-6 strong-600 mt-2 mb-3"
                              >
                                <router-link :to="`/posts/${post.id}`">{{
                                  post.blurb
                                }}</router-link>
                              </h2>
                            </div>
                          </div>
                        </div>
                      </div>
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
      message: "Welcome to User show page!",
      currentUser: {},
      updateUserParams: {},
      editable: false,
    };
  },
  created: function () {
    axios.get("/users/me").then((response) => {
      console.log(response.data);
      this.currentUser = response.data;
    });
  },
  methods: {
    updateUser: function () {
      var updateUserParams = this.currentUser;
      axios
        .patch("/users/me", updateUserParams)
        .then((response) => {
          console.log(response.data);
          this.editable = false;
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
  },
};
</script>
