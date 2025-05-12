<template>
  <div class="max-w-4xl mx-auto p-6">
    <h1 class="text-2xl font-bold mb-4">Checkout</h1>

    <!-- Cart Items -->
    <div v-for="item in cartItems" :key="item.id" class="flex justify-between border-b py-2">
      <div>{{ item.product.name }} x {{ item.quantity }}</div>
      <div>Rp {{ item.product.price * item.quantity }}</div>
    </div>

    <!-- Customer Info -->
    <div class="mt-6 space-y-4">
      <input v-model="form.name" type="text" placeholder="Nama" class="w-full border p-2" />
      <input v-model="form.phone" type="text" placeholder="Nomor Telepon" class="w-full border p-2" />
      <textarea v-model="form.address" placeholder="Alamat" class="w-full border p-2"></textarea>

      <!-- Provinsi & Kota -->
      <div class="grid grid-cols-2 gap-4">
        <select v-model="form.province_id" @change="fetchCities" class="w-full border p-2">
          <option disabled value="">Pilih Provinsi</option>
          <option v-for="prov in provinces" :key="prov.province_id" :value="prov.province_id">
            {{ prov.province }}
          </option>
        </select>

        <select v-model="form.city_id" class="w-full border p-2">
          <option disabled value="">Pilih Kota</option>
          <option v-for="city in cities" :key="city.city_id" :value="city.city_id">
            {{ city.city_name }}
          </option>
        </select>
      </div>

      <!-- Kurir -->
      <select v-model="form.courier" @change="fetchShippingCost" class="w-full border p-2">
        <option disabled value="">Pilih Kurir</option>
        <option value="jne">JNE</option>
        <option value="pos">POS</option>
        <option value="tiki">TIKI</option>
      </select>
    </div>

    <!-- Ongkir & Total -->
    <div class="mt-6">
      <p>Ongkir: Rp {{ shippingCost || '-' }}</p>
      <p class="text-xl font-bold">Total: Rp {{ total + (shippingCost || 0) }}</p>
    </div>

    <!-- Submit -->
    <button @click="submitOrder" class="mt-4 px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700">
      Buat Pesanan
    </button>
  </div>
</template>

<script setup>
import { onMounted, reactive, ref, computed } from 'vue'
import { useStore } from 'vuex'
import { useRouter } from 'vue-router'
import axios from '@/plugins/axios'

const store = useStore()
const router = useRouter()

const cartItems = computed(() => store.state.cart.items)
const total = computed(() => store.getters['cart/totalPrice'])

const form = reactive({
  name: '',
  phone: '',
  address: '',
  province_id: '',
  city_id: '',
  courier: ''
})

const provinces = ref([])
const cities = ref([])
const shippingCost = ref(0)

onMounted(() => {
  fetchProvinces()
})

const fetchProvinces = async () => {
  const res = await axios.get('/api/rajaongkir/provinces')
  provinces.value = res.data
}

const fetchCities = async () => {
  const res = await axios.get(`/api/rajaongkir/cities/${form.province_id}`)
  cities.value = res.data
}

const fetchShippingCost = async () => {
  const weight = cartItems.value.reduce((sum, item) => sum + item.quantity * 500, 0) // asumsi 500g per item
  const res = await axios.post('/api/rajaongkir/shipping-cost', {
    origin: 501, // ID kota asal toko
    destination: form.city_id,
    weight,
    courier: form.courier
  })
  shippingCost.value = res.data.cost
}

const submitOrder = async () => {
  try {
    await axios.post('/api/checkout', {
      ...form,
      shipping_cost: shippingCost.value,
      total: total.value + shippingCost.value,
      items: cartItems.value
    })
    alert('Pesanan berhasil dibuat!')
    store.commit('cart/clearCart')
    router.push('/')
  } catch (err) {
    alert('Gagal membuat pesanan.')
    console.error(err)
  }
}
</script>
