<template>
  <div class="card border-0 rounded shadow-sm border-top-orange">
    <div class="card-body">
      <h5>MAIN MENU</h5>
      <hr>
      <ul class="list-group">
        <nuxt-link
          :to="{ name: 'customer-dashboard' }"
          exact
          class="list-group-item text-decoration-none text-dark text-uppercase"
        >
          <i class="fa fa-tachometer-alt me-2"></i> Dashboard
        </nuxt-link>

        <nuxt-link
          :to="{ name: 'customer-invoices' }"
          class="list-group-item text-decoration-none text-dark text-uppercase"
        >
          <i class="fa fa-shopping-cart me-2"></i> My Orders
        </nuxt-link>

        <a
          @click="logout"
          class="list-group-item text-decoration-none text-dark text-uppercase"
          style="cursor: pointer;"
        >
          <i class="fa fa-sign-out-alt me-2"></i> Logout
        </a>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    async logout() {
      try {
        await this.$auth.logout()

        this.$store.commit('web/cart/SET_CARTS_DATA', [])
        this.$store.commit('web/cart/SET_CART_PRICE', 0)

        this.$router.push({ name: 'customer-login' })
      } catch (error) {
        console.error('Logout failed:', error)
      }
    },
  },
}
</script>

<style scoped>
a.nuxt-link-active {
  background: rgba(255, 222, 212, 0.1) !important;
  font-weight: bold;
}
.list-group-item:hover {
  background-color: #f9f9f9;
  transition: all 0.2s ease-in-out;
}
</style>
