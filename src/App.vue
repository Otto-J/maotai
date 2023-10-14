<script setup lang="ts">
import { onMounted, ref, watchEffect } from 'vue'
import { Icon } from '@iconify/vue'
import { useQRCode } from '@vueuse/integrations/useQRCode'
// import html2canvas from 'html2canvas'

const _h1 = location.search.match(/h1=([^&]+)/)
const _h2 = location.search.match(/h2=([^&]+)/)
// console.log(3, _h1, _h2)

const h1 = ref(decodeURIComponent(_h1?.[1] ?? '') || '周五来了')
const h2 = ref(decodeURIComponent(_h2?.[1] ?? '') || 'Web Worker提醒您')
const url = ref('https://api.webworker.tech/s?id=ww36')
const showQrcode = ref(true)
const showHeader = ref(true)

const toggleHeader = () => {
  showHeader.value = !showHeader.value
}
const toggleQrcode = () => {
  showQrcode.value = !showQrcode.value
}

const share = () => {
  // 把 h1 h2 拼接 url
  const url = location.origin + location.pathname + '?h1=' + h1.value + '&h2=' + h2.value
  console.log(url)
  location.href = url
}
const downloadImage = () => {
  showHeader.value = false
}

const resetForm = () => {
  h1.value = '周五来了'
  h2.value = 'Web Worker提醒您'
  url.value = ''
}

const showModal = ref(false)
const closeModal = () => {
  showModal.value = false
}
const qrcode = ref('')
onMounted(() => {
  qrcode.value = useQRCode(url.value).value
})

const goPodcast = () => {
  window.open('https://api.webworker.tech/s?id=ww36')
}
watchEffect(() => {
  console.log(h1.value)
  // if (!url.value) {
  //   return
  // }
  // const r = useQRCode(url.value)
  // qrcode.value = r.value
})
</script>

<template>
  <header v-show="showHeader" class="fixed top-0 left-0 w-full z-10 bg-black">
    <div class="max-w-7xl mx-auto px-4 sm:px-2 lg:px-8">
      <div class="flex items-center justify-between py-3">
        <div class="flex-shrink-0">
          <!-- <img class="h-8 w-8" src="logo.png" alt="Logo" /> -->
          <span class="text-white ml-2">MT</span>
        </div>
        <!-- content -->
        <div class="flex-grow flex flex-row flex-nowrap text-xs justify-around">
          <div class="flex justify-start items-center space-x-2">
            <span> {{ h2 }} / {{ h1 }} </span>
            <Icon icon="line-md:edit" height="16" @click="showModal = true"></Icon>
          </div>
        </div>
        <!-- github -->
        <div class="flex-shrink-0">
          <a href="https://github.com" class="">
            <Icon icon="grommet-icons:github" height="24"></Icon>
          </a>
        </div>
      </div>
    </div>
  </header>
  <div id="container" class="relative w-full h-full box max-w-xl mx-auto overflow-hidden">
    <div class="box2 flex-col h-full flex justify-center items-center">
      <h2
        id="sub-title"
        class="sub-title w-full text-center font-bold text-3xl py-2 italic whitespace-pre"
        style="background-color: var(--mt-brown); color: var(--mt-dark-blue)"
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
        class="main-title bg-white w-full text-center text-6xl py-2 font-extrabold italic whitespace-pre"
        style="color: var(--mt-red)"
        @click="toggleHeader"
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
    <!-- v-if="qrcode.trim() === ''" -->

    <img
      v-if="false"
      @click="toggleQrcode"
      :src="qrcode"
      alt="QR Code"
      :style="{
        opacity: showQrcode ? 1 : 0
      }"
      style="position: absolute; top: 66%; left: 67%; width: 120px"
    />
  </div>

  <footer v-show="showHeader" class="fixed bottom-0 left-0 right-0 py-3 bg-black">
    <div class="max-w-7xl mx-auto px-4 sm:px-2 lg:px-8 flex justify-between items-center">
      <Icon @click="share" icon="mdi:share" height="30" />
      <span class="text-sm" @click="goPodcast">前端程序员都爱听 Web Worker 播客</span>
      <Icon icon="mdi:download" height="30" @click="downloadImage" />
    </div>
  </footer>

  <div id="modal" v-show="showModal" class="fixed inset-0 flex items-center justify-center z-50">
    <div class="bg-white rounded-sm p-8 relative opacity-90">
      <div class="absolute z-10 right-3 top-3" @click="closeModal">
        <Icon color="var(--mt-dark-blue)" icon="grommet-icons:close" height="18" />
      </div>
      <div class="mb-4 color-dark-blue">
        <label class="block color-dark-blue text-sm font-bold mb-2" for="input1">Sub Title</label>
        <input
          class="w-full px-4 py-2 rounded-md border-gray-300 focus:ring-indigo-500 focus:border-indigo-500"
          type="text"
          id="input1"
      
          v-model="h2"
          placeholder="请输入内容"
        />
      </div>
      <div class="mb-4">
        <label class="text-sm color-dark-blue font-bold mb-2" for="input2">Main Title</label>
        <input
          class="outline-1 color-dark-blue w-full px-4 py-2 rounded-md border-gray-300 focus:ring-indigo-500 focus:border-indigo-500"
          type="text"
          id="input2"
  
          v-model="h1"
          placeholder="请输入内容"
        />
        <span class="text-sm text-gray-400">Click to hide header and footer</span>
      </div>
      <div class="mb-4" v-if="false">
        <label class="text-sm color-dark-blue font-bold mb-2" for="input3">QRCode</label>
        <input
          class="outline-1 color-dark-blue w-full px-4 py-2 rounded-md border-gray-300 focus:ring-indigo-500 focus:border-indigo-500"
          type="text"
          id="input3"
          v-model="url"
          placeholder="请输入内容"
        />
        <span class="text-sm text-gray-400">Click QRCode will hide itself</span>
      </div>
      <div class="flex justify-end">
        <button
          class="bg-gray-500 hover:bg-gray-700 text-white py-2 px-4 rounded mr-2"
          @click="resetForm"
        >
          <Icon icon="grommet-icons:power-reset" height="18" />
        </button>
        <button
          class="bg-blue-500 hover:bg-blue-700 text-white py-2 px-4 rounded mr-2"
          @click="closeModal"
        >
          <Icon icon="grommet-icons:checkmark" height="18" />
        </button>
        <!-- <button class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded">
          关闭
        </button> -->
      </div>
    </div>
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
  margin-left: -50%;
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
.color-dark-blue {
  color: var(--mt-dark-blue);
}
#modal input {
  border: 1px solid var(--mt-brown);
  color: var(--mt-dark-blue);
}
</style>
