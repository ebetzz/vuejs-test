<template>
  <div class="home">
    <Navbar />
    <div class="row">
      <div class="col">
        <img src="../assets/ilustrasi.png" width="100%" />
      </div>
      <div class="col text-center">
        <h1>Selamat Datang</h1>
        <br />
        di coba vue .(dot) com
        <br />
        Tunggu kami di hari yang sama
      </div>
    </div>
    <div class="row mt-3">
      <div class="col-md-4 mt-3" v-for="p in Products" :key="p.id">
        <CardProduct :p="p" />
      </div>
    </div>
  </div>
</template>

<script>
// // @ is an alias to /src
import Navbar from "@/components/Navbar.vue";
import CardProduct from "@/components/CardProduct.vue";
import axios from "axios";

export default {
  name: "Home",
  components: {
    CardProduct,
    Navbar,
  },
  data() {
    return {
      Products: [],
    };
  },
  methods: {
    setProducts(data) {
      this.Products = data;
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/best-products")
      .then((response) => this.setProducts(response.data))
      .catch((error) => console.log(error));
  },
};
</script>
