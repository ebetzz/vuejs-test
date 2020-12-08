<template>
  <div class="FoodDetail">
    <Navbar />
    <div class="row mt-3">
      <div class="col">breadcrumb</div>
    </div>
    <div class="row mt-3">
      <div class="col-md-6">
        <img
          :src="`${publicPath}assets/${p.gambar}`"
          class="img-fluid shadow rounded float-right"
        />
      </div>
      <div class="col-md-6">
        <H3>{{ p.nama }}</H3>
        <hr />
        Harga : <br />
        <strong> Rp.{{ p.harga | currency }}</strong>
        <form class="mt-2" v-on:submit.prevent>
          <div class="form-group">
            <label for="jumlah_pemesanan">Jumlah Pemesanan</label>
            <input
              type="number"
              placeholder="1"
              class="form-control"
              v-model="pesan.jumlah_pemesanan"
            />
          </div>
          <div class="form-group">
            <label for="keterangan">Keterangan</label>
            <textarea
              class="form-control"
              placeholder="keterangan misal: pedas, nasi setengah..."
              v-model="pesan.keterangan"
            ></textarea>
          </div>
          <button
            class="btn btn-success float-left"
            type="submit"
            @click="pemesanan"
          >
            <b-icon-cart></b-icon-cart> pesan
          </button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Navbar from "@/components/Navbar.vue";

export default {
  name: "FoodDetail",
  components: {
    Navbar,
  },
  data() {
    return {
      publicPath: process.env.BASE_URL,
      p: [],
      pesan: {},
    };
  },
  methods: {
    setProduct(data) {
      this.p = data;
    },
    pemesanan() {
      if (this.pesan.jumlah_pemesanan > 0) {
        this.pesan.products = this.p;
        axios
          .post("http://localhost:3000/keranjangs", this.pesan)
          .then(() => {
            this.$router.push({ path: "/cart" });
            this.$toast.success("Berhasil masuk keranjang", {
              type: "success",
              position: "top-right",
              duration: 5000,
              dismissable: true,
            });
          })
          .catch((error) => console.log(error));
      } else {
        this.$toast.error("Jumlah pesanan minimal 1", {
          type: "error",
          position: "top-right",
          duration: 5000,
          dismissable: true,
        });
      }
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/products/" + this.$route.params.id)
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log(error));
  },
};
</script>