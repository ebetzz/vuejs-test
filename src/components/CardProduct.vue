<template>
  <div id="CardProduct">
    <div class="card shadow" style="width: 18rem">
      <img :src="require(`../../assets/${p.gambar}`)" class="card-img-top" />
      <div class="card-body">
        <H3 class="card-text">{{ p.nama }}</H3>
        Hargan : Rp. <strong> {{ p.harga | currency }} </strong><br />
        <router-link :to="'/menu/' + p.id" class="btn btn-success float-right"
          ><b-icon-cart></b-icon-cart> Pesan
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CardProduct",
  props: ["p"],
};
</script>
