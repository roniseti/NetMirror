<script setup>
import { ref, onMounted, computed, watch } from 'vue'
import { useMotion } from '@vueuse/motion'
import { useNodeTool } from '@/composables/useNodeTool'
import FileSpeedtest from '@/components/Speedtest/FileSpeedtest.vue'
import Librespeed from '@/components/Speedtest/Librespeed.vue'

const cardRef = ref()

const {
  selectedNode,
  selectedNodeName,
  selectedNodeLocation
} = useNodeTool()

// 获取当前节点的配置 - 使用节点的真实配置
const currentConfig = computed(() => {
  if (selectedNode.value && selectedNode.value.config) {
    return selectedNode.value.config
  }
  return {}
})

const availableTests = computed(() => {
  const tests = []
  const config = currentConfig.value
  if (config?.feature_librespeed) {
    tests.push({ id: 'librespeed', name: 'Librespeed' })
  }
  if (config?.feature_filespeedtest) {
    tests.push({ id: 'filespeedtest', name: 'File-based Test' })
  }
  return tests
})

const activeTest = ref(null)

// 监听availableTests变化，自动设置第一个可用测试
watch(availableTests, (newTests) => {
  if (newTests.length > 0 && !activeTest.value) {
    activeTest.value = newTests[0].id
  }
}, { immediate: true })

const { apply } = useMotion(cardRef, {
  initial: { opacity: 0, y: 20 },
  enter: { opacity: 1, y: 0, transition: { duration: 500, delay: 400 } }
})

onMounted(() => {
  if (availableTests.value.length > 0) {
    apply()
  }
})
</script>

<template>
  <div 
    v-if="availableTests.length > 0"
    ref="cardRef" 
    class="bg-white dark:bg-gray-900 backdrop-blur-sm rounded-xl border dark:border-gray-700 overflow-hidden"
  >
    
    <div class="p-6 space-y-6">
      <!-- Sub-tabs for speed tests -->
      <!-- <div v-if="availableTests.length > 1" class="flex justify-center">
        <div class="bg-primary-100/50 dark:bg-gray-700/50 rounded-lg p-1 flex space-x-1">
          <button
            v-for="test in availableTests"
            :key="test.id"
            @click="activeTest = test.id"
            :class="[
              'px-4 py-2 text-sm font-medium rounded-md transition-colors duration-200 focus:outline-none',
              activeTest === test.id
                ? 'bg-primary-600 text-white shadow-lg'
                : 'text-gray-600 dark:text-gray-300 hover:bg-primary-50 dark:hover:bg-gray-600/50 hover:text-primary-700 dark:hover:text-white'
            ]"
          >
            {{ test.name }}
          </button>
        </div>
      </div> -->
      <!-- Sub-tabs for speed tests -->
      <div v-if="availableTests.length > 1" class="flex justify-center">
        <div class="inline-flex rounded-md border border-gray-200 dark:border-gray-700 bg-gray-100 dark:bg-gray-800 p-1">
          <button
            v-for="test in availableTests"
            :key="test.id"
            @click="activeTest = test.id"
            :class="[
              'px-3 py-1.5 text-sm rounded-sm transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-gray-400 focus-visible:ring-offset-2 focus-visible:ring-offset-white dark:focus-visible:ring-offset-gray-900',
              activeTest === test.id
                ? 'bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100 shadow-sm'
                : 'text-gray-500 dark:text-gray-400 hover:text-gray-900 dark:hover:text-gray-100'
            ]"
          >
            {{ test.name }}
          </button>
        </div>
      </div>

      <!-- Conditionally rendered speed test components -->
      <div>
        <Librespeed v-if="activeTest === 'librespeed'" />
        <FileSpeedtest v-if="activeTest === 'filespeedtest'" />
      </div>
    </div>
  </div>
  
  <!-- No node selected state -->
  <div v-else class="bg-white dark:bg-gray-900 backdrop-blur-sm rounded-xl border dark:border-gray-700 overflow-hidden">
    <div class="p-6 text-center">
      <div class="w-16 h-16 mx-auto mb-4 bg-gray-100 dark:bg-gray-700 rounded-full flex items-center justify-center">
        <svg class="w-8 h-8 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path>
        </svg>
      </div>
      <h3 class="text-lg font-medium text-gray-700 dark:text-gray-300 mb-2">Speed Test</h3>
      <p class="text-gray-500 dark:text-gray-400">Please select a node to run speed tests.</p>
    </div>
  </div>
</template>
