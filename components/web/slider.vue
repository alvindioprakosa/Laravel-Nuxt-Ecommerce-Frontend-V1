<template>
  <div class="container-fluid mt-4">
    <div id="mycarousel" class="carousel slide" data-ride="carousel" data-interval="4000">
      <div class="carousel-inner">
        <div
          class="carousel-item"
          v-for="(slider, id) in sliders"
          :class="{ active: id === 0 }"
          :key="slider.id"
        >
          <a :href="slider.link" target="_blank" rel="noopener noreferrer">
            <img
              :src="slider.image || defaultImage"
              class="d-block w-100 rounded"
              alt="Slider Image"
            />
          </a>
        </div>
      </div>

      <a class="carousel-control-prev" href="#mycarousel" role="button" data-slide="prev">
        <div class="banner-icons">
          <span class="fa fa-angle-left"></span>
        </div>
        <span class="sr-only">Previous</span>
      </a>
      <a class="carousel-control-next" href="#mycarousel" role="button" data-slide="next">
        <div class="banner-icons">
          <span class="fa fa-angle-right"></span>
        </div>
        <span class="sr-only">Next</span>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      defaultImage: '/images/default-slider.jpg', // path fallback image jika tidak ada slider.image
    };
  },

  async fetch() {
    // Ambil data slider dari Vuex store
    await this.$store.dispatch('web/slider/getSlidersData');
  },

  computed: {
    sliders() {
      return this.$store.state.web.slider.sliders;
    },
  },
};
</script>

<style scoped>
.carousel-item img {
  max-height: 500px;
  object-fit: cover;
}

.banner-icons {
  background-color: rgba(0, 0, 0, 0.4);
  padding: 10px;
  border-radius: 50%;
  color: white;
  font-size: 2rem;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
