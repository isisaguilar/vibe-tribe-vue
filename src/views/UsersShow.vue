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
                    <a href="#" class="btn-aux">
                      <i class="ion ion-edit"></i>
                    </a>
                  </div>

                  <!-- Profile details -->
                  <div class="profile-details mb-4">
                    <h2 class="heading heading-6 strong-600 profile-name">
                      {{ currentUser.name }}
                    </h2>
                  </div>

                  <hr />

                  <ul class="categories categories--style-3 mt-3">
                    <li>
                      <a href="/html/e-commerce/account-settings.html">
                        <i class="ion-gear-b"></i>
                        <span class="category-name"> Settings </span>
                      </a>
                    </li>
                    <li>
                      <a
                        href="/html/e-commerce/account-orders.html"
                        class="active"
                      >
                        <i class="ion-calendar"></i>
                        <span class="category-name"> Orders </span>
                      </a>
                    </li>
                    <li>
                      <a href="/html/e-commerce/account-wishlist.html">
                        <i class="ion-heart"></i>
                        <span class="category-name"> Wishlist </span>
                      </a>
                    </li>
                    <li>
                      <a href="/html/e-commerce/account-notifications.html">
                        <i class="ion-email-unread"></i>
                        <span class="category-name"> Notifications </span>
                      </a>
                    </li>
                  </ul>
                </div>
              </div>
            </div>

            <div class="col-lg-9">
              <div class="main-content">
                <!-- Page title -->
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
                        <h4 href="#" class="link text-underline--none">
                          My Posts
                        </h4>
                      </h2>
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
                      <div
                        class="row cols-md-space cols-sm-space cols-xs-space"
                      >
                        <div
                          v-for="post in currentUser.posts"
                          v-bind:key="post.id"
                          class="col-lg-6 mb-5"
                        >
                          <div class="col-lg-5">
                            <div class="card card-product">
                              <div class="card-body">
                                <div class="card-image swiper-js-container">
                                  <div class="">
                                    <div
                                      class="swiper-container"
                                      data-swiper-items="1"
                                      data-swiper-space-between="0"
                                    >
                                      <div class="swiper-wrapper">
                                        <div
                                          v-if="post.image_url"
                                          class="swiper-slide"
                                        >
                                          <img
                                            :src="post.image_url"
                                            alt=""
                                            class="
                                              img-fluid img-center img-primary
                                            "
                                          />
                                        </div>
                                        <div
                                          v-if="post.video_url"
                                          class="swiper-slide"
                                        >
                                          <img
                                            :src="post.video_url"
                                            alt=""
                                            class="
                                              img-fluid img-center img-primary
                                            "
                                          />
                                        </div>
                                      </div>
                                    </div>
                                  </div>
                                </div>
                                <div v-if="post.blurb">
                                  <h2
                                    class="
                                      heading heading-6
                                      strong-600
                                      mt-2
                                      mb-3
                                    "
                                  >
                                    {{ post.blurb }}
                                  </h2>
                                </div>
                                <div v-if="post.blog">
                                  <h2
                                    class="
                                      heading heading-6
                                      strong-600
                                      mt-2
                                      mb-3
                                    "
                                  >
                                    {{ post.blog }}
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
        </div>
      </div>
    </section>

    <h1>{{ message }}</h1>
    <h2>
      {{ currentUser.name }} <br />
      <img :src="currentUser.image_url" alt="" />
    </h2>
    <form v-on:submit.prevent="updateUser()">
      <h1>User Info</h1>
      <div>Name: <input type="text" v-model="currentUser.name" /></div>

      <br />
      <div>Email: <input type="text" v-model="currentUser.email" /></div>

      <br />
      <div>
        Profile Image: <input type="text" v-model="currentUser.image_url" />
      </div>
      <button type="submit">Update</button>
      <p>{{ updateUserParams }}</p>
      <br />
    </form>
    <div v-for="post in currentUser.posts" v-bind:key="post.id">
      <span v-if="post.blurb"
        >Blurb: <br />
        {{ post.blurb }}</span
      >
      <br />
      <span v-if="post.blog"
        >Blog: <br />
        {{ post.blog }}</span
      >
      <br />
      <span v-if="post.image_url">
        Image: <br />
        <img :src="post.image_url" alt=""
      /></span>
      <br />
      <span v-if="post.video_url">
        Video: <br />
        <img :src="post.video_url" alt=""
      /></span>
    </div>
    <!-- <h3>{{ currentUser }}</h3> -->
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
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
  },
};
</script>
