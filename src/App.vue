<script setup lang="ts">
import { ref } from 'vue'
import { useQRCode } from '@vueuse/integrations/useQRCode'

const h1 = ref('周五来了')
const h2 = ref('    Web Worker提醒您')
const url = ref('https://api.webworker.tech/s?id=ww36')

const qrcode = useQRCode(url.value)
</script>

<template>
  <div class="relative w-full h-full box">
    <div class="box2 flex-col h-full flex justify-center items-center">
      <h2
        id="sub-title"
        class="sub-title w-full text-center font-bold text-3xl py-2 italic"
        style="background-color: var(--mt-brown); color: var(--mt-dark-blue); margin-left: 23vw"
      >
        {{ h2 }}
      </h2>
      <div class="w-full space-y-2 py-2" style="background-color: var(--mt-dark-blue)">
        <div class="h-1 bg-white w-full"></div>
        <div class="h-1 bg-white w-full"></div>
        <div class="h-1 bg-white w-full"></div>
        <div class="h-1 bg-white w-full"></div>
      </div>
      <h1
        class="main-title bg-white w-full text-center text-6xl py-2 font-extrabold italic"
        style="color: var(--mt-red)"
      >
        {{ h1 }}
      </h1>
      <div class="w-full space-y-2 py-2" style="background-color: var(--mt-dark-blue)">
        <div class="h-1 bg-white w-full"></div>
        <div class="h-1 bg-white w-full"></div>
        <div class="h-1 bg-white w-full"></div>
        <div class="h-1 bg-white w-full"></div>
      </div>
      <div class="w-full h-3" style="background-color: var(--mt-brown)"></div>
    </div>
    <img
      :src="qrcode"
      alt="QR Code"
      style="position: absolute; top: 66%; left: 67%; width: 120px"
    />
  </div>
</template>

<style>
.box {
  background-color: var(--mt-red);
}
.box2 {
  transform: rotate(-28deg);
  width: 200%;
  transform-origin: center;
  margin-left: -50vw;
}
h2.sub-title {
  /* -webkit-text-stroke: 1px #fff; */
}
h1.main-title {
  /* font-size: 6em; */
  -webkit-text-stroke: 3px var(--mt-brown);
  font-size: 100px;
  /* text-stroke: 1px #fff; */
}
</style>
