<script setup lang="ts">
import denah from '@/assets/images/image.png'
import lamp_on from '@/assets/images/lamp_on.png'
import lamp_off from '@/assets/images/lamp_off.png'
import { onMounted, ref, shallowRef } from 'vue'
import goodJob from '@/assets/sounds/lucky-star-good-job.mp3'
import { useToast } from 'primevue'
import axios from 'axios'
import type { AxiosResponse } from 'axios'

type LampStatus = {
  lamp1: boolean
  lamp2: boolean
  lamp3: boolean
  lamp4: boolean
}

const API_URL = 'http://192.168.18.37'

// fetch status lamps
onMounted(async () => {
  try {
    loading.value = true
    const { data } = (await axios.get(`${API_URL}/status`)) as AxiosResponse<LampStatus>
    lampStatus.value = data
  } catch (error) {
    toast.add({
      severity: 'error',
      summary: 'Error',
      detail: 'Failed to fetch lamp status',
      life: 3000
    })
  } finally {
    loading.value = false
  }
})

const toast = useToast()
const loading = shallowRef(false)
const lampStatus = ref<LampStatus>({
  lamp1: false,
  lamp2: false,
  lamp3: false,
  lamp4: false
})

const playSound = (sound: string) => {
  const audio = new Audio(sound)
  audio.play().catch((error) => {
    console.error('Error playing sound:', error)
  })
}

const toggleLamp = async (room: keyof LampStatus) => {
  try {
    loading.value = true
    const payload: LampStatus = {
      ...lampStatus.value,
      [room]: !lampStatus.value[room]
    }
    // Send the updated status to the server
    // await axios.post(`${API_URL}/status`, payload)

    const status = lampStatus.value[room] ? 'on' : 'off'
    playSound(goodJob)
    lampStatus.value = payload
    toast.add({
      severity: 'success',
      summary: 'Success',
      detail: `Lamp in ${room} turned ${status} successfully!`,
      life: 3000
    })
  } catch (error) {
    console.error('Error toggling lamp:', error)
    toast.add({
      severity: 'error',
      summary: 'Error',
      detail: 'Failed to toggle lamp status',
      life: 3000
    })
  } finally {
    loading.value = true
  }
}
</script>

<template>
  <div
    class="relative w-full min-h-67 sm:min-h-187.5 mt-5 rounded bg-cover bg-center"
    :style="{ backgroundImage: `url(${denah})` }"
  >
    <!-- Loading overlay -->
    <div
      v-if="loading"
      class="absolute inset-0 bg-black bg-opacity-50 flex items-center justify-center z-10 rounded"
    >
      <div class="flex flex-col items-center">
        <div class="spinner">
          <div class="double-bounce1"></div>
          <div class="double-bounce2"></div>
        </div>
        <p class="text-white mt-3 font-medium">Memuat...</p>
      </div>
    </div>

    <img
      :src="lampStatus.lamp1 ? lamp_on : lamp_off"
      alt="pemanasan"
      class="absolute w-15 h-15 cursor-pointer"
      @click="toggleLamp('lamp1')"
      style="top: 30%; left: 25%"
      :class="{ 'opacity-70': loading }"
    />

    <img
      :src="lampStatus.lamp4 ? lamp_on : lamp_off"
      alt="penyucian"
      class="absolute w-15 h-15 cursor-pointer"
      style="top: 50%; left: 60%"
      @click="toggleLamp('lamp4')"
      :class="{ 'opacity-70': loading }"
    />

    <img
      :src="lampStatus.lamp2 ? lamp_on : lamp_off"
      alt="penyetrikaan"
      class="absolute w-15 h-15 cursor-pointer"
      style="top: 55%; left: 20%"
      @click="toggleLamp('lamp2')"
      :class="{ 'opacity-70': loading }"
    />

    <img
      :src="lampStatus.lamp3 ? lamp_on : lamp_off"
      alt="Toilet"
      class="absolute w-15 h-15 cursor-pointer top-35 left-125"
      @click="toggleLamp('lamp3')"
      :class="{ 'opacity-70': loading }"
    />
  </div>
</template>

<style scoped>
.spinner {
  width: 40px;
  height: 40px;
  position: relative;
}

.double-bounce1,
.double-bounce2 {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background-color: #ffffff;
  opacity: 0.6;
  position: absolute;
  top: 0;
  left: 0;
  animation: sk-bounce 2s infinite ease-in-out;
}

.double-bounce2 {
  animation-delay: -1s;
}

@keyframes sk-bounce {
  0%,
  100% {
    transform: scale(0);
  }
  50% {
    transform: scale(1);
  }
}
</style>
