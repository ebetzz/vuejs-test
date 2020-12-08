<template>
  <div class="cart">
    <Navbar :refreshCart="keranjangs" />
    <div class="row">
      <div class="col">
        <h2>My <strong>Cart</strong></h2>
        <br />
        <div class="table-responsive mt-3">
          <table class="table">
            <thead>
              <tr>
                <th scope="col">#</th>
                <th scope="col">Foto</th>
                <th scope="col">Makanan</th>
                <th scope="col">Harga</th>
                <th scope="col">Jumlah</th>
                <th scope="col">Keterangan</th>
                <th scope="col">Total Harga</th>
                <th scope="col">Hapus</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(k, index) in keranjangs" :key="k.id">
                <td>
                  <strong>{{ index + 1 }}</strong>
                </td>
                <td>
                  <img
                    :src="require(`../../assets/${k.products.gambar}`)"
                    class="img-fluid shadow rounded"
                    width="250"
                  />
                </td>
                <td>{{ k.products.nama }}</td>
                <td align="right">
                  Rp. <strong>{{ k.products.harga | currency }}</strong>
                </td>
                <td>{{ k.jumlah_pemesanan }}</td>
                <td>{{ k.keterangan ? k.keterangan : "-" }}</td>
                <td align="right">
                  Rp.
                  <strong>{{
                    (k.jumlah_pemesanan * k.products.harga) | currency
                  }}</strong>
                </td>
                <td align="center" class="text-danger">
                  <b-icon-trash @click="hapusKeranjang(k.id)"></b-icon-trash>
                </td>
              </tr>
              <tr>
                <td colspan="6" align="right">
                  <strong>Total Harga :</strong>
                </td>
                <td align="right">
                  Rp. <strong>{{ totalHarga | currency }}</strong>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="float-right">
          <form class="mt-2" v-on:submit.prevent>
            <div class="form-group">
              <label for="nama_pemesan">Nama</label>
              <input type="text" class="form-control" v-model="pesan.nama" />
            </div>
            <div class="form-group">
              <label for="alamat_pemesan">Alamat</label>
              <textarea
                class="form-control"
                placeholder="Jalan Kebun Raya Bogor"
                v-model="pesan.alamat"
              ></textarea>
            </div>
            <button
              class="btn btn-success float-left"
              type="submit"
              @click="pemesanan"
            >
              <b-icon-cart></b-icon-cart> Checkout
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "Cart",
  data() {
    return {
      keranjangs: [],
      pesan: {},
    };
  },
  components: {
    Navbar,
  },
  props: {},
  methods: {
    setKeranjang(data) {
      this.keranjangs = data;
    },
    hapusKeranjang(id) {
      axios
        .delete("http://localhost:3000/keranjangs/" + id)
        .then(() => {
          this.$toast.warning("Berhasil hapus keranjang", {
            type: "success",
            position: "top-right",
            duration: 5000,
            dismissable: true,
          });
          axios
            .get("http://localhost:3000/keranjangs")
            .then((response) => this.setKeranjang(response.data))
            .catch((error) => console.log(error));
        })
        .catch((error) => console.log(error));
    },
    pemesanan() {
      if (this.pesan.nama && this.pesan.alamat) {
        this.pesan.keranjangs = this.keranjangs;
        axios
          .post("http://localhost:3000/pesanans", this.pesan)
          .then(() => {
            this.keranjangs.map(function (item) {
              return axios
                .delete("http://localhost:3000/keranjangs/" + item.id)
                .catch((error) => console.log(error));
            });

            this.$router.push({ path: "/menu" });
            this.$toast.success("Memesan", {
              type: "success",
              position: "top-right",
              duration: 5000,
              dismissable: true,
            });
          })
          .catch((error) => console.log(error));
      } else {
        this.$toast.error("Mohon isi field yang dibutuhkan", {
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
      .get("http://localhost:3000/keranjangs")
      .then((response) => this.setKeranjang(response.data))
      .catch((error) => console.log(error));
  },
  computed: {
    totalHarga() {
      return this.keranjangs.reduce(function (items, data) {
        return items + data.products.harga * data.jumlah_pemesanan;
      }, 0);
    },
  },
};
</script>