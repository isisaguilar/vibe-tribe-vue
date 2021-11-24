<template>
  <div id="app">
    <!-- Header -->
    <div class="header">
      <!-- Top Bar -->

      <!-- Navbar -->
      <nav class="navbar navbar-expand-lg navbar--bold navbar-light bg-base-1">
        <div class="container navbar-container">
          <!-- Brand/Logo -->
          <router-link to="/posts" class="navbar-brand">
            <blockquote class="blockquote blockquote--style-1 mt-4 mb-4">
              Vibe Tribe
            </blockquote>
          </router-link>

          <div class="d-inline-block">
            <!-- Navbar toggler  -->
            <button
              class="navbar-toggler hamburger hamburger-js hamburger--spring"
              type="button"
              data-toggle="collapse"
              data-target="#navbar_main"
              aria-controls="navbarsExampleDefault"
              aria-expanded="false"
              aria-label="Toggle navigation"
            >
              <span class="hamburger-box">
                <span class="hamburger-inner"></span>
              </span>
            </button>
          </div>

          <div
            class="
              collapse
              navbar-collapse
              align-items-center
              justify-content-end
            "
            id="navbar_main"
          >
            <ul class="navbar-nav ml-lg-auto">
              <li v-if="isLoggedIn()" class="nav-item">
                <router-link to="/users/me" class="nav-link">
                  Profile
                </router-link>
              </li>
              <li v-if="isLoggedIn()" class="nav-item">
                <router-link to="/boards" class="nav-link">
                  Boards
                </router-link>
              </li>
              <li class="nav-item">
                <router-link to="/posts" class="nav-link"> Posts </router-link>
              </li>
              <li v-if="isLoggedIn()" class="nav-item">
                <router-link to="/logout" class="nav-link">
                  Logout
                </router-link>
              </li>
              <li v-if="!isLoggedIn()" class="nav-item">
                <router-link to="/login" class="nav-link"> Login </router-link>
              </li>
            </ul>
          </div>

          <div v-if="!isLoggedIn()" class="pl-4 d-none d-lg-inline-block">
            <router-link
              to="/signup"
              class="btn btn-styled btn-sm btn-base-1 btn-circle"
            >
              Signup
            </router-link>
          </div>
        </div>
      </nav>
    </div>
    <router-view />
  </div>
</template>

<script>
import Darkmode from "darkmode-js";

const options = {
  bottom: "64px", // default: '32px'
  right: "unset", // default: '32px'
  left: "32px", // default: 'unset'
  time: "0.5s", // default: '0.3s'
  mixColor: "#fff", // default: '#fff'
  backgroundColor: "#fff", // default: '#fff'
  buttonColorDark: "#100f2c", // default: '#100f2c'
  buttonColorLight: "#fff", // default: '#fff'
  saveInCookies: false, // default: true,
  label: "🌓", // default: ''
  autoMatchOsTheme: true, // default: true
};

const darkmode = new Darkmode(options);
darkmode.showWidget();

export default {
  data: function () {
    return {};
  },
  methods: {
    isLoggedIn: function () {
      return localStorage.jwt;
    },
    getUserId: function () {
      return localStorage.user_id;
    },
  },
};
</script>
