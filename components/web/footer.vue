<template>
  <footer class="pt-5" style="background-color: #fff; border-top: 5px solid #e64a1a;">
    <div class="container-fluid">
      <div class="row">
        <!-- Tentang -->
        <div class="col-md-5 mb-4">
          <h4 class="font-weight-bold">TENTANG</h4>
          <hr class="bg-light" style="height: 3px; border-radius: 0.5rem;">
          <p>
            Mi Store Official terpercaya di Indonesia. Jual beli aman &amp; harga termurah! Belanja sekarang.
          </p>
        </div>

        <!-- Metode Pembayaran -->
        <div class="col-md-3 mb-4">
          <h4 class="font-weight-bold">METODE PEMBAYARAN</h4>
          <hr class="bg-light" style="height: 3px; border-radius: 0.5rem;">
          <div class="row">
            <div
              v-for="(image, index) in paymentImages"
              :key="index"
              class="col-4 mb-3"
            >
              <div class="card rounded">
                <div class="card-body p-2 text-center">
                  <img :src="image.src" :alt="image.alt" :style="{ width: image.width }">
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Jam Operasional -->
        <div class="col-md-4 mb-4">
          <h4 class="font-weight-bold">JAM OPERASIONAL</h4>
          <hr class="bg-light" style="height: 3px; border-radius: 0.5rem;">
          <p>
            <i class="fa fa-clock"></i> Toko buka setiap hari:
            <br><br>
            <strong>Senin - Jum'at</strong> (07.00 - 19.00)
            <br>
            <strong>Sabtu - Minggu</strong> (07.00 - 16.00)
          </p>
        </div>
      </div>

      <!-- Copyright -->
      <div class="row text-center mt-3 pb-3">
        <div class="col-12">
          <hr>
          © <strong>MI STORE</strong> 2021 • Hak Cipta Dilindungi
        </div>
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  data() {
    return {
      paymentImages: [
        { src: "/images/payment/BCA.png", alt: "BCA", width: "50px" },
        { src: "/images/payment/BNI.png", alt: "BNI", width: "45px" },
        { src: "/images/payment/BRI.png", alt: "BRI", width: "60px" },
        { src: "/images/payment/GOPAY.png", alt: "Gopay", width: "60px" },
        { src: "/images/payment/indomaret-logo.png", alt: "Indomaret", width: "60px" },
        { src: "/images/payment/atm-bersama.jpg", alt: "ATM Bersama", width: "40px" }
      ]
    }
  }
}
</script>

<style scoped>
.card img {
  max-height: 40px;
  object-fit: contain;
}
</style>
