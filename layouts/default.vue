<template>
  <>
    <Header />
    <Nuxt />
    <Footer />
  </>
</template>

<script>
import Header from '@/components/web/header.vue'
import Footer from '@/components/web/footer.vue'

export default {
  components: { Header, Footer }
}
</script>

<style scoped>
/* Tambahkan styling jika diperlukan */
</style>
