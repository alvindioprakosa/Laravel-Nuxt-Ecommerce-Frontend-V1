<template>
  <div class="container-fluid mt-custom">
    <div class="fade-in">
      <div class="row">
        <div class="col-md-12">
          <h3>
            PENCARIAN DENGAN KATA KUNCI :
            <strong>{{ $route.query.q }}</strong>
          </h3>
          <hr class="solid" />
        </div>
      </div>

      <div class="row" v-if="products.data.length > 0">
        <div
          class="col-md-3 mt-1 mb-4"
          v-for="product in products.data"
          :key="product.id"
        >
          <div class="card h-100 border-0 rounded shadow-sm">
            <div class="card-body">
              <div class="card-img-actions">
                <img
                  :src="product.image"
                  class="card-img img-fluid"
                  loading="lazy"
                  :alt="product.title"
                />
              </div>
            </div>
            <div class="card-body bg-light-custom text-center rounded-bottom">
              <div class="mb-2">
                <h6 class="font-weight-semibold mb-2">
                  <nuxt-link
                    :to="{
                      name: 'products-slug',
                      params: { slug: product.slug },
                    }"
                    class="text-default mb-2"
                    data-abc="true"
                    >{{ product.title }}</nuxt-link
                  >
                </h6>
                <nuxt-link
                  :to="{
                    name: 'categories-slug',
                    params: { slug: product.category.slug },
                  }"
                  class="text-muted"
                  data-abc="true"
                  >{{ product.category.name }}</nuxt-link
                >
              </div>
              <h6 class="mb-0 font-weight-semibold">
                <s class="text-red">Rp. {{ formatPrice(product.price) }}</s> /
                <strong>{{ product.discount }}%</strong>
              </h6>
              <h5 class="mb-0 font-weight-semibold mt-3 text-success">
                Rp. {{ formatPrice(calculateDiscount(product)) }}
              </h5>
              <hr />
              <client-only>
                <vue-star-rating
                  :rating="parseFloat(product.reviews_avg_rating)"
                  :increment="0.5"
                  :star-size="20"
                  :read-only="true"
                  :show-rating="false"
                  :inline="true"
                />
                (<strong>{{ product.reviews_count }}</strong> ulasan)
              </client-only>
            </div>
          </div>
        </div>
      </div>

      <div class="row justify-content-center" v-else>
        <div class="col-md-12">
          <b-alert show variant="danger">DATA PRODUK TIDAK DITEMUKAN!</b-alert>
        </div>
      </div>

      <div class="row justify-content-center mt-4 mb-4">
        <div class="text-center">
          <b-pagination
            align="center"
            :value="products.current_page"
            :total-rows="products.total"
            :per-page="products.per_page"
            @change="changePage"
            aria-controls="product-pagination"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  head() {
    const keyword = this.$route.query.q || "Pencarian";
    return {
      title: `Pencarian untuk : ${keyword} - MI STORE - Distributor Xiaomi Indonesia Resmi`,
      meta: [
        {
          hid: "description",
          name: "description",
          content: `Temukan produk Xiaomi terbaik dengan kata kunci "${keyword}" hanya di MI STORE Indonesia.`,
        },
      ],
    };
  },

  watchQuery: ["q"],

  async asyncData({ store, query }) {
    await store.dispatch("web/product/getProductsData", query.q);
  },

  computed: {
    products() {
      return this.$store.state.web.product.products;
    },
  },

  methods: {
    changePage(page) {
      this.$store.commit("web/product/SET_PAGE", page);
      this.$store.dispatch(
        "web/product/getProductsData",
        this.$route.query.q
      );
    },

    formatPrice(value) {
      return Number(value).toLocaleString("id-ID");
    },

    calculateDiscount(product) {
      const price = Number(product.price);
      const discount = Number(product.discount);
      return price - (price * discount) / 100;
    },
  },
};
</script>

<style scoped>
.mt-custom {
  margin-top: 2rem;
}
hr.solid {
  border-top: 2px solid #bbb;
}
.text-red {
  color: red;
}
.bg-light-custom {
  background-color: #f8f9fa;
}
</style>
