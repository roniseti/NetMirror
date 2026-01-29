<!-- <template>
  <div class="flex items-start justify-between gap-4">
    <div>
      <div class="text-xs text-gray-500 dark:text-gray-400 mb-1">
        {{ label }}
      </div>
      <div
        class="font-mono text-sm text-gray-900 dark:text-gray-100 break-all"
        :class="highlight && 'text-primary-600 dark:text-primary-400'"
      >
        {{ value }}
      </div>
    </div>

    <button
      v-if="copyable"
      @click="copyToClipboard(value, $event.target)"
      class="shrink-0 rounded-md p-2 text-gray-400 hover:text-gray-600
             dark:hover:text-gray-200 transition-colors"
    >
      <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
          d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z" />
      </svg>
    </button>
  </div>
</template> -->
<template>
  <div class="flex items-start justify-between gap-4">
    <div class="min-w-0">
      <!-- Label -->
      <div class="text-sm text-gray-500 dark:text-gray-400 mb-1">
        {{ label }}
      </div>

      <!-- Value -->
      <div
        class="font-mono text-base font-medium break-all leading-relaxed"
        :class="[
          highlight
            ? 'text-primary-600 dark:text-primary-400'
            : 'text-gray-900 dark:text-gray-100'
        ]"
      >
        {{ value }}
      </div>
    </div>

    <!-- Copy button -->
    <!-- <button
      v-if="copyable && value"
      @click="onCopy"
      class="shrink-0 mt-1 rounded-md p-2
             text-gray-400 hover:text-gray-600
             dark:hover:text-gray-200
             transition-colors"
      aria-label="Copy"
    >
      <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"
        />
      </svg>
    </button> -->
  </div>
</template>


<script setup>
const props = defineProps({
  label: { type: String, required: true },
  value: { type: [String, Number], default: '' },
  copyable: { type: Boolean, default: false },
  highlight: { type: Boolean, default: false }
})

// const emit = defineEmits(['copy'])

// async function onCopy(e) {
//   await navigator.clipboard.writeText(props.value)

//   emit('copy', props.value, e.target)
// }
</script>