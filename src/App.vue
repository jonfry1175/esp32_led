<script setup lang="ts">
import haitod from '@/assets/sounds/haitod.mp3'
import MainView from './views/MainView.vue'
import { ConfirmDialog, Toast } from 'primevue'

const handleOpenDialogConfirm = () => {
  const audio = new Audio(haitod)
  audio.currentTime = 1
  audio.play()
}
</script>

<template>
  <Toast />
  <ConfirmDialog @show="handleOpenDialogConfirm" />
  <MainView />
</template>
