<template>
  <div class="relative w-full">
    <UTextarea
      v-model="model"
      :rows="rows"
      :placeholder="placeholder"
      class="pr-12"
    />

    <UTooltip :text="copied ? 'Скопировано' : 'Копировать'">
      <button
        @click="copy"
        class="absolute right-2 top-2 bg-gray-200 dark:bg-gray-700 rounded p-1
               transition hover:opacity-100 opacity-80 flex items-center justify-center"
      >
        <UIcon
          :name="copied ? 'i-heroicons-check-20-solid' : 'i-heroicons-clipboard-20-solid'"
          class="w-5 h-5"
        />
      </button>
    </UTooltip>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  modelValue: String,
  rows: { type: Number, default: 6 },
  placeholder: String,
})

const emit = defineEmits(['update:modelValue'])

const copied = ref(false)
let timer = null

const model = computed({
  get: () => props.modelValue,
  set: v => emit('update:modelValue', v)
})

async function copy() {
  await navigator.clipboard.writeText(model.value || '')

  copied.value = true
  clearTimeout(timer)
  timer = setTimeout(() => (copied.value = false), 1500)
}
</script>
