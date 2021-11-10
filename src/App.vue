<template>
  <div id="app">
    <!-- Header -->
    <div class="header">
      <!-- Top Bar -->

      <!-- Navbar -->
      <nav class="navbar navbar-expand-lg navbar--bold navbar-light bg-default">
        <div class="container navbar-container">
          <!-- Brand/Logo -->
          <a class="navbar-brand" href="/index.html">
            <img
              src="/assets/images/logo/logo-1-b.png"
              class=""
              alt="Boomerang"
            />
          </a>

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
