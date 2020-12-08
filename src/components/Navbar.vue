 <template id="navBar">
  <div class="Navbar">
    <nav
      class="navbar navbar-expand-lg navbar-light"
      v-bind:class="{ navbarOpen: show }"
    >
      <div class="container">
        <router-link class="navbar-brand" to="/">
          <img src="../assets/skrzlogoonly.png" width="30" />
          <strong> SKRZ </strong>
        </router-link>
        <button
          class="navbar-toggler"
          type="button"
          data-toggle="collapse"
          data-target="#navbarNav"
          aria-controls="navbarNav"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon" @click="toggleNavbar"></span>
        </button>
        <div
          class="collapse navbar-collapse"
          id="navbarNav"
          v-bind:class="{ show: show }"
        >
          <ul class="navbar-nav mr-auto">
            <li class="nav-item">
              <router-link class="nav-link" to="/">Home</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/about">About</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/menu">Menu</router-link>
            </li>
          </ul>
          <ul class="navbar-nav ml-auto">
            <li class="nav-item">
              <router-link class="nav-link" to="/cart">
                Cart <b-icon-cart></b-icon-cart>
                <span class="badge badge-success">
                  {{ refreshCart ? refreshCart.length : jumlah_pesanan.length }}
                </span>
              </router-link>
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Navbar",
  component: "navBar",
  template: "#navBar",
  props: {
    refreshCart: {},
    msg: String,
  },
  data() {
    return {
      jumlah_pesanan: [],
      show: true,
    };
  },
  methods: {
    setJumlah(data) {
      this.jumlah_pesanan = data;
    },
    toggleNavbar() {
      this.show = !this.show;
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      .then((response) => this.setJumlah(response.data))
      .catch((error) => console.log(error));
  },
};
</script>

<style>
</style>