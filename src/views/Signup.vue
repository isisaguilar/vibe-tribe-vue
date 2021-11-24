<template>
  <div class="signup">
    <section
      class="slice-lg has-bg-cover bg-size-cover"
      style="
        background-image: url('https://images.unsplash.com/photo-1501862700950-18382cd41497?ixid=MnwxMjA3fDB8MHxzZWFyY2h8OHx8dW5pdmVyc2V8ZW58MHx8MHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=800&q=60');
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
                    Join the Vibe Tribe
                  </h4>
                </div>

                <form class="form-default mt-4" v-on:submit.prevent="submit()">
                  <ul>
                    <li v-for="error in errors" v-bind:key="error">
                      {{ error }}
                    </li>
                  </ul>
                  <div class="row">
                    <div class="col-md-6">
                      <div class="form-group">
                        <label class="control-label">Name</label>
                        <input
                          type="text"
                          class="form-control form-control-lg"
                          v-model="newUserParams.name"
                        />
                      </div>
                    </div>

                    <div class="col-md-6">
                      <div class="form-group">
                        <label class="control-label">Email</label>
                        <input
                          type="email"
                          class="form-control form-control-lg"
                          v-model="newUserParams.email"
                        />
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
                          v-model="newUserParams.image_url"
                        />
                      </div>
                    </div>
                  </div>

                  <div class="row">
                    <div class="col-md-6">
                      <div class="form-group">
                        <label class="control-label">Password</label>
                        <input
                          type="password"
                          class="form-control form-control-lg"
                          v-model="newUserParams.password"
                        />
                      </div>
                    </div>
                    <div class="col-md-6">
                      <div class="form-group has-feedback">
                        <label class="control-label">Confirm Password</label>
                        <input
                          type="password"
                          class="form-control form-control-lg"
                          v-model="newUserParams.password_confirmation"
                        />
                      </div>
                    </div>
                  </div>

                  <button
                    type="submit"
                    class="btn btn-styled btn-base-1 btn-circle px-4 mt-5"
                  >
                    Signup
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
      newUserParams: {},
      errors: [],
    };
  },
  methods: {
    submit: function () {
      axios
        .post("/users", this.newUserParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push("/login");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
