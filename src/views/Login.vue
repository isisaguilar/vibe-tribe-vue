<template>
  <div class="login">
    <section
      class="slice-lg has-bg-cover bg-size-cover"
      style="
        background-image: url(/assets/images/backgrounds/slider/img-52.jpg);
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
                    Sign in to your account
                  </h4>
                </div>

                <form class="form-default mt-4" v-on:submit.prevent="submit()">
                  <ul>
                    <li v-for="error in errors" v-bind:key="error">
                      {{ error }}
                    </li>
                  </ul>
                  <div class="row">
                    <div class="col-md-12">
                      <div class="form-group">
                        <label class="control-label">Email</label>
                        <input
                          type="email"
                          class="form-control form-control-lg"
                          v-model="newSessionParams.email"
                        />
                      </div>
                    </div>
                  </div>
                  <div class="row">
                    <div class="col-md-12">
                      <div class="form-group">
                        <label class="control-label">Password</label>
                        <input
                          type="password"
                          class="form-control form-control-lg"
                          v-model="newSessionParams.password"
                        />
                      </div>
                    </div>
                  </div>
                  <button type="submit" class="btn btn-styled btn-base-1 mt-4">
                    Login
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
      newSessionParams: {},
      errors: [],
    };
  },
  methods: {
    submit: function () {
      axios.post("/sessions", this.newSessionParams).then((response) => {
        axios.defaults.headers.common["Authorization"] =
          "Bearer " + response.data.jwt;
        localStorage.setItem("jwt", response.data.jwt);
        localStorage.setItem("user_id", response.data.user_id);
        this.$router.push("/users/me");
      });
    },
  },
};
</script>
