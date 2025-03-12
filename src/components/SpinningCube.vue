<script setup>
import { TresCanvas } from '@tresjs/core'
import { useRenderLoop } from '@tresjs/core'
import { ref } from 'vue'

const cubeRotation = ref({ x: 0, y: 0 })

useRenderLoop(() => {
  cubeRotation.value.x += 0.01
  cubeRotation.value.y += 0.01
})
</script>

<template>
  <TresCanvas window-size>
    <TresAmbientLight :intensity="1" />
    <TresDirectionalLight :position="[2, 2, 2]" :intensity="1" />
    <TresMesh :rotation="[cubeRotation.x, cubeRotation.y, 0]">
      <TresBoxGeometry :args="[1, 1, 1]" />
      <TresMeshStandardMaterial color="#4169e1" />
    </TresMesh>
  </TresCanvas>
</template>

<style scoped>
canvas {
  width: 100% !important;
  height: 300px !important;
  background: transparent;
}
</style> 