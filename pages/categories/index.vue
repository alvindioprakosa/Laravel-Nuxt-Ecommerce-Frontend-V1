<template>
  <div class="container-fluid mt-custom">
    <div class="fade-in">
      <div class="row justify-content-center">
        <div
          class="col-md-3 mb-3"
          v-for="category in categories"
          :key="category.id"
        >
          <div class="card border-0 rounded shadow-sm h-100">
            <div class="card-body text-center">
              <nuxt-link
                :to="{ name: 'categories-slug', params: { slug: category.slug } }"
                class="text-decoration-none text-dark"
              >
                <img
                  :src="category.image"
                  alt="Kategori {{ category.name }}"
                  width="100"
                  class="mb-2"
                />
                <hr />
                <h5 class="mb-0">{{ category.name }}</h5>
              </nuxt-link>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  head() {
    const title = 'Semua Kategori - MI STORE - Distributor Xiaomi Indonesia Resmi'
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
          content: '/images/logo.png'
        },
        {
          hid: 'description',
          name: 'description',
          content: 'Semua Kategori dari Produk Xiaomi secara resmi dari MI STORE Indonesia'
        }
      ]
    }
  },

  async asyncData({ store }) {
    await store.dispatch('web/category/getCategoriesData')
  },

  computed: {
    categories() {
      return this.$store.state.web.category.categories
    }
  }
}
</script>

<style scoped>
.mt-custom {
  margin-top: 80px;
}
img {
  max-width: 100%;
  height: auto;
}
</style>
