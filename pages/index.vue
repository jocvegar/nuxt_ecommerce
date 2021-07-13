<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <v-card class="logo py-4 d-flex flex-column align-center">
        <div class="d-flex  justify-center my-10">
          <NuxtLogo />
          <VuetifyLogo />
        </div>

        <v-card-title class="headline my-10">
          Welcome to the JocFy
        </v-card-title>
        <v-card-text>
          <div>
            <h1>{{ merchant.business_name }}</h1>
            <br />
            <h3><NuxtLink to="/categories">Categories NuxtLink</NuxtLink></h3>
            <category-list :categories="categories"></category-list>
            <hr />
            <h3><nuxt-link to="/products">Products nuxt-link </nuxt-link></h3>
            <product-list :products="products"></product-list>
          </div>
        </v-card-text>
        <v-card-text>
          <h1>cliente side</h1>
          <div v-if="loading === false">
            <pre>{{ JSON.stringify(categorias, null, 2) }}</pre>
          </div>
        </v-card-text>
      </v-card>
      <hr class="my-10" />
      <Tutorial />
    </v-col>
  </v-row>
</template>

<script>
import commerce from "~/common/commerce";

export default {
  head: {
    titleTemplate: "%s | Ella Uso Mi Cabeza"
  },
  data() {
    return {
      loading: true,
      categorias: [],
      productos: []
    };
  },
  mounted() {
    this.fetcheData();
  },
  methods: {
    async fetcheData() {
      const merchant = await commerce.merchants.about();
      const _categoias = await commerce.categories.list();
      this.categorias = _categoias.data;
      this.loading = false;
    }
  },
  async asyncData({ $commerce }) {
    const merchant = await $commerce.merchants.about();
    const { data: categories } = await $commerce.categories.list();
    const { data: products } = await $commerce.products.list();

    return {
      merchant,
      categories,
      products
    };
  }
};
</script>
