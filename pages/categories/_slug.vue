<template>
  <div class="container-fluid mt-custom">
    <div class="fade-in">
      <div class="row">
        <div class="col-md-12">
          <h3>CATEGORY: <strong>{{ category.name?.toUpperCase() }}</strong></h3>
          <hr class="solid" />
        </div>
      </div>
      <div class="row">
        <div
          class="col-md-3 mt-1 mb-4"
          v-for="product in category.products"
          :key="product.id"
        >
          <div class="card h-100 border-0 rounded shadow-sm">
            <div class="card-body">
              <div class="card-img-actions">
                <img
                  :src="product.image"
                  class="card-img img-fluid"
                  alt="Gambar {{ product.title }}"
                />
              </div>
            </div>
            <div class="card-body bg-light-custom text-center rounded-bottom">
              <div class="mb-2">
                <h6 class="font-weight-semibold mb-2">
                  <nuxt-link
                    :to="{ name: 'products-slug', params: { slug: product.slug } }"
                    class="text-default mb-2"
                    data-abc="true"
                  >
                    {{ product.title }}
                  </nuxt-link>
                </h6>
                <nuxt-link
                  :to="{ name: 'categories-slug', params: { slug: product.category.slug } }"
                  class="text-muted"
                  data-abc="true"
                >
                  {{ product.category.name }}
                </nuxt-link>
              </div>
              <h6 class="mb-0 font-weight-semibold">
                <s class="text-danger">Rp. {{ formatPrice(product.price) }}</s>
                / <strong>{{ product.discount }}%</strong>
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
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ store, route }) {
    await store.dispatch('web/category/getDetailCategory', route.params.slug)
  },

  head() {
    const title = `Category : ${this.category?.name || 'Loading...'} - MI STORE - Distributor Xiaomi Indonesia Resmi`
    return {
      title,
      meta: [
        {
          hid: 'og:title',
          name: 'og:title',
          content: title
        },
        {
          hid: 'og:site_name',
          name: 'og:site_name',
          content: 'MI STORE Indonesia'
        },
        {
          hid: 'og:image',
          name: 'og:image',
          content: this.category?.image || '/images/logo.png'
        },
        {
          hid: 'description',
          name: 'description',
          content: `Lihat produk-produk dalam kategori ${this.category?.name || ''} di MI STORE Indonesia`
        }
      ]
    }
  },

  computed: {
    category() {
      return this.$store.state.web.category.category || { products: [] }
    }
  },

  methods: {
    formatPrice(value) {
      if (!value) return '0'
      return Number(value).toLocaleString('id-ID')
    },
    calculateDiscount(product) {
      const discount = (product.price * product.discount) / 100
      return product.price - discount
    }
  }
}
</script>

<style scoped>
.mt-custom {
  margin-top: 80px;
}
.card-img {
  max-height: 200px;
  object-fit: contain;
}
.solid {
  border-top: 2px solid #bbb;
}
.bg-light-custom {
  background-color: #f9f9f9;
}
</style>
