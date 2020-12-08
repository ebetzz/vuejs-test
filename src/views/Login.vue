<template>
  <div class="Login">
    <div class="row justify-content-center">
      <div class="card o-hidden border-0 shadow-lg my-5">
        <div class="col">
          <div class="p-5">
            <div class="text-center">
              <h1 class="h4 mb-4">Welcome, Login First</h1>
            </div>
            <form v-on:submit.prevent>
              <div class="form-group">
                <!--<input type="text" class="form-control form-control-user" id="email" name="email" placeholder="Enter Email Address...">-->
                <input
                  type="text"
                  class="form-control form-control-user"
                  id="email"
                  v-model="email"
                  required
                  name="email"
                />
                <div v-if="email.length < 1" class="text-danger">
                  Email harus diisi
                </div>
              </div>
              <div class="form-group">
                <!--<input type="password" class="form-control form-control-user" id="password" name="password" placeholder="Password">-->
                <input
                  type="password"
                  class="form-control form-control-user"
                  id="password"
                  v-model="password"
                  required
                  name="password"
                />
                <div v-if="password.length < 1" class="text-danger">
                  Password harus diisi
                </div>
              </div>
              <button
                type="submit"
                name="login"
                class="btn btn-primary btn-user btn-block"
                @click="login"
              >
                Login
              </button>
              <div class="form-group mt-3">
                <div class="custom-control custom-checkbox small">
                  <input
                    type="checkbox"
                    class="custom-control-input"
                    id="remember"
                    name="remember"
                  />
                  <label class="custom-control-label" for="remember"
                    >Remember Me</label
                  >
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import qs from "qs";

export default {
  name: "Login",
  data() {
    return {
      email: "",
      password: "",
      Login: [],
    };
  },
  components: {},
  methods: {
    setLogin(data) {
      this.Login = data;
    },

    login() {
      if (this.email && this.password) {
        // kudu pake stringify kyknya...
        var prepdata = qs.stringify({
          name: this.email,
          pass: this.password,
        });

        //axios postnya
        axios({
          method: "post",
          url: "http://localhost:8080/Auth/login",
          // url: "http://localhost/ci_rest/api/login/",
          headers: {
            // "x-API": "testkey",
            // Authorization: "Basic a29wbGFrMTIzOnNhbXBhaDMyMTQ1",
            "Content-Type": "application/x-www-form-urlencoded",
          },
          data: prepdata,
        })
          .then((response) => {
            // this.setLogin(response.data);
            // console.log(response.data);
            localStorage.setItem("user-token", response.data.token);
            localStorage.setItem("user", response.data.user);
            // let rep = response.data;
            // console.log(rep.length);
            // if (response.data.length > 0) {
            //   console.log("lebih dari 1");
            // } else {
            //   console.log("kosong");
            // }
            // console.log("email : " + this.email);
            // console.log("Password : " + this.password);
            this.$router.push({ path: "/" });
          })
          .catch((error) => {
            localStorage.removeItem("user-token");
            console.log("ini catch nya");
            console.log(error);
          });
      } else {
        console.log("Harus di isi");
      }
    },
  },
};
</script>

<style>
</style>