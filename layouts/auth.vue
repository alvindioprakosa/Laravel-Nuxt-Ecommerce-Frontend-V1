<template>
  <div class="c-app d-flex flex-row align-items-center">
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-md-4">
          <Nuxt />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Pastikan font hanya diterapkan di komponen ini jika tidak digunakan di global */
body {
  font-family: "Quicksand", sans-serif;
  font-size: initial !important;
}

.form-control {
  height: auto;
  font-size: 1rem;
}

.c-app {
  background-color: #bccad8 !important;
}
</style>
