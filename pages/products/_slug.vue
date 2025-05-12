<template>
  <div class="container mt-custom mb-5">
    <div class="fade-in">
      <div class="row">
        <!-- Gambar Produk -->
        <div class="col-md-4 mb-4">
          <div class="card border-0 rounded shadow-sm">
            <div class="card-body">
              <img :src="product.image" class="w-100 rounded" alt="product image" />
            </div>
          </div>
        </div>

        <!-- Detail Produk -->
        <div class="col-md-8">
          <div class="card border-0 rounded shadow-sm">
            <div class="card-body">
              <h4>{{ product.title }}</h4>
              <hr />
              <h6 class="mb-0 font-weight-semibold">
                <s class="text-red">Rp. {{ formatPrice(product.price) }}</s> /
                <strong>{{ product.discount }}%</strong>
              </h6>
              <h5 class="mb-0 font-weight-semibold mt-3 text-success">
                Rp. {{ formatPrice(calculateDiscount(product)) }}
              </h5>
              <div class="mt-3" v-html="product.description"></div>

              <div class="table-responsive">
                <table class="table table-sm table-borderless mb-0">
                  <tbody>
                    <client-only>
                      <tr>
                        <th class="pl-0 w-25"><strong>BERAT</strong></th>
                        <td><strong>{{ product.weight }}</strong> gram</td>
                      </tr>
                      <tr>
                        <th class="pl-0 w-25"><strong>STOK</strong></th>
                        <td><strong>{{ product.stock }}</strong></td>
                      </tr>
                    </client-only>
                  </tbody>
                </table>
              </div>

              <hr />
              <button
                @click="addToCart(product.id, calculateDiscount(product), product.weight)"
                class="btn btn-lg btn-warning border-0 shadow-sm"
              >
                <i class="fa fa-shopping-cart"></i> TAMBAH KE KERANJANG
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- Ulasan Produk -->
      <div class="row mt-4">
        <div class="col-md-12">
          <div class="card border-0 rounded shadow-sm">
            <div class="card-body">
              <h5>
                <i class="fa fa-comments"></i> ULASAN PRODUK (
                <strong>{{ product.reviews_count }}</strong> ulasan )
              </h5>
              <hr />
              <div
                class="card bg-light shadow-sm rounded mb-3"
                v-for="review in product.reviews"
                :key="review.id"
              >
                <div class="card-body">
                  <div class="row">
                    <div class="col-md-1">
                      <div class="review-avatar avatar-sm">
                        <img
                          :src="`https://ui-avatars.com/api/?name=${review.customer.name}&background=4e73df&color=ffffff&size=100`"
                          alt="avatar"
                        />
                      </div>
                    </div>
                    <div class="col-md-11">
                      <client-only>
                        <vue-star-rating
                          class="mb-2"
                          :rating="review.rating"
                          :star-size="20"
                          :read-only="true"
                          :show-rating="false"
                        />
                      </client-only>
                      <strong class="text-dark">{{ review.customer.name }}</strong>
                      <div class="description mt-2 text-muted" style="font-size:15px;font-style:italic">
                        {{ review.review }}
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <!-- End Ulasan -->
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { formatPrice, calculateDiscount } from '@/utils/price';

export default {
  async asyncData({ store, route, error }) {
    try {
      await store.dispatch('web/product/getDetailProduct', route.params.slug);
    } catch (err) {
      error({ statusCode: 404, message: 'Produk tidak ditemukan' });
    }
  },

  computed: {
    product() {
      return this.$store.state.web.product.product;
    },
  },

  head() {
    const title = this.product?.title || 'Produk';
    return {
      title: `${title} - MI STORE - Distributor Xiaomi Indonesia Resmi`,
      meta: [
        { hid: 'og:title', name: 'og:title', content: title },
        { hid: 'og:site_name', name: 'og:site_name', content: title },
        { hid: 'og:image', name: 'og:image', content: this.product?.image },
        { hid: 'description', name: 'description', content: title.substr(0, 30) + '...' }
      ]
    };
  },

  methods: {
    formatPrice,
    calculateDiscount,

    async addToCart(productId, price, weight) {
      if (!this.$auth.loggedIn || this.$auth.strategy.name !== 'customer') {
        return this.$router.push({ name: 'customer-login' });
      }

      try {
        await this.$store.dispatch('web/cart/storeCart', {
          product_id: productId,
          price,
          qty: 1,
          weight,
        });

        this.$swal.fire({
          title: 'BERHASIL!',
          text: 'Product Berhasil Ditambahkan di Keranjang!',
          icon: 'success',
          showConfirmButton: false,
          timer: 3000,
        });
      } catch (e) {
        console.error('Gagal menambahkan ke keranjang:', e);
        this.$swal.fire({
          title: 'GAGAL',
          text: 'Terjadi kesalahan saat menambahkan ke keranjang',
          icon: 'error',
        });
      }
    }
  }
};
</script>
