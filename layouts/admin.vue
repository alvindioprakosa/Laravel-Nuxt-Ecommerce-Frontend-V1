<template>
  <div class="c-app">
    <!-- Sidebar -->
    <aside class="c-sidebar c-sidebar-dark c-sidebar-fixed c-sidebar-lg-show">
      <div class="c-sidebar-brand d-lg-down-none">
        <img
          src="/images/xiaomi.png"
          class="bg-light rounded shadow-sm p-2"
          width="35"
          alt="MI STORE Logo"
        />
        <span class="ml-2 font-weight-bold">{{ brandName }}</span>
      </div>

      <Sidebar />
    </aside>

    <!-- Wrapper -->
    <div class="c-wrapper c-fixed-components">
      <Header />

      <main class="c-body">
        <Nuxt />

        <!-- Footer -->
        <footer class="c-footer">
          <div><strong>{{ brandName }}</strong> &copy; {{ currentYear }} - SantriKoding.com.</div>
          <div class="ml-auto">
            Template by <a href="https://coreui.io/" target="_blank" rel="noopener">CoreUI</a>
          </div>
        </footer>
      </main>
    </div>
  </div>
</template>

<script>
import Header from "@/components/admin/header.vue";
import Sidebar from "@/components/admin/sidebar.vue";

export default {
  middleware: "isAdmin",

  components: {
    Header,
    Sidebar,
  },

  data() {
    return {
      brandName: "MI STORE",
      currentYear: new Date().getFullYear(),
    };
  },
};
</script>

<style scoped>
/* Tambahkan scoped agar tidak mengganggu style global */
</style>