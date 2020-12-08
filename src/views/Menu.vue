<template>
  <div class="Menu">
    <Navbar />
    <div class="row mt-3">
      <div class="col-md-8">
        <div class="input-group">
          <input
            v-model="cari"
            type="text"
            class="form-control"
            placeholder="Cari disini"
            @keyup="searchfood"
          />
          <div class="input-group-prepend">
            <span class="input-group-text" id="basic-addon1"
              ><b-icon-search></b-icon-search
            ></span>
          </div>
        </div>
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
  name: "Menu",
  components: {
    CardProduct,
    Navbar,
  },
  data() {
    return {
      Products: [],
      cari: "",
    };
  },
  methods: {
    setProducts(data) {
      this.Products = data;
    },
    searchfood() {
      axios
        .get("http://localhost:3000/products?q=" + this.cari)
        .then((response) => this.setProducts(response.data))
        .catch((error) => console.log(error));
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/products")
      .then((response) => this.setProducts(response.data))
      .catch((error) => console.log(error));
  },
};
</script>