<script setup lang="ts">
import DefaultLayout from '@/layouts/DefaultLayout.vue'
import denah from '@/assets/images/denah.jpg'
import lamp_on from '@/assets/images/lamp_on.png'
import lamp_off from '@/assets/images/lamp_off.png'
import { ref } from 'vue'

// Menggunakan shallowRef dengan generic untuk status lampu
type LampStatus = {
  bedroom: boolean
  prayerRoom: boolean
  bathroom: boolean
}

const lampStatus = ref<LampStatus>({
  bedroom: true,
  prayerRoom: true,
  bathroom: true
})

// Fungsi untuk toggle status lampu
const toggleLamp = (room: keyof LampStatus) => {
  lampStatus.value = {
    ...lampStatus.value,
    [room]: !lampStatus.value[room]
  }
}
</script>

<template>
  <DefaultLayout>
    <div
      class="relative w-full h-[1100px] mt-5 rounded bg-cover bg-center"
      :style="{ backgroundImage: `url(${denah})` }"
    >
      <!-- Lampu ruang tidur -->
      <img
        :src="lampStatus.bedroom ? lamp_on : lamp_off"
        alt="Lampu Tidur"
        class="absolute w-15 h-15 cursor-pointer"
        style="top: 65%; left: 60%"
        @click="toggleLamp('bedroom')"
      />

      <!-- Lampu ruang ibadah -->
      <img
        :src="lampStatus.prayerRoom ? lamp_on : lamp_off"
        alt="Lampu Ibadah"
        class="absolute w-15 h-15 cursor-pointer"
        style="top: 50%; left: 29%"
        @click="toggleLamp('prayerRoom')"
      />

      <!-- Lampu kamar mandi -->
      <img
        :src="lampStatus.bathroom ? lamp_on : lamp_off"
        alt="Lampu Kamar Mandi"
        class="absolute w-15 h-15 cursor-pointer"
        style="top: 15%; left: 60%"
        @click="toggleLamp('bathroom')"
      />
    </div>
  </DefaultLayout>
</template>
