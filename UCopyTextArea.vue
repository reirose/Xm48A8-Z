<!-- components/UCopyTextarea.vue -->
<template>
  <div class="relative w-full">
    <UTextarea
      v-model="model"
      :rows="rows"
      :placeholder="placeholder"
      class="pr-12"
    />

    <button
      @click="copy"
      class="absolute right-2 top-2 bg-gray-200 dark:bg-gray-700 rounded px-2 py-1
             text-sm opacity-80 hover:opacity-100 transition"
    >
      Copy
    </button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  modelValue: String,
  rows: { type: Number, default: 6 },
  placeholder: { type: String, default: '' }
})
const emit = defineEmits(['update:modelValue'])

const model = computed({
  get: () => props.modelValue,
  set: v => emit('update:modelValue', v)
})

async function copy() {
  await navigator.clipboard.writeText(model.value)
}
</script>
