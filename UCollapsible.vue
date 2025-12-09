<template>
  <component
    :is="wrapperTag"
    :class="wrapperClasses"
  >
    <button
      class="flex justify-between w-full px-4 py-3 items-center text-left select-none"
      @click="toggle"
    >
      <div class="font-medium">
        <slot name="label" />
      </div>

      <svg
        :style="{ transform: open ? 'rotate(180deg)' : 'rotate(0deg)' }"
        class="transition-transform duration-200"
        width="18"
        height="18"
        viewBox="0 0 24 24"
      >
        <path stroke="currentColor" stroke-width="2" fill="none"
              d="M6 9l6 6 6-6"/>
      </svg>
    </button>

    <transition name="collapse" @enter="enter" @leave="leave">
      <div v-show="open" ref="content" class="overflow-hidden">
        <div class="px-4 pb-3 pt-1">
          <slot />
        </div>
      </div>
    </transition>
  </component>
</template>

<script setup>
import { computed, ref } from 'vue'

defineProps({
  variant: {
    type: String,
    default: 'default' // default | ghost | card
  },
  rounded: { type: Boolean, default: true },
  shadow: { type: Boolean, default: false }
})

const open = ref(false)
const content = ref(null)
const toggle = () => open.value = !open.value

const wrapperTag = computed(() => variant === 'card' ? 'UCard' : 'div')

const wrapperClasses = computed(() => {
  if (variant === 'ghost') {
    return 'overflow-hidden'
  }

  const base = []
  if (rounded) base.push('rounded-md')
  if (shadow) base.push('shadow')
  
  return variant === 'card'
    ? base.join(' ')
    : 'border ' + base.join(' ')
})

// height animation
function enter(el) {
  el.style.height = '0'
  requestAnimationFrame(() => {
    el.style.transition = 'height .25s ease'
    el.style.height = el.scrollHeight + 'px'
  })
  el.addEventListener('transitionend', () => {
    el.style.height = 'auto'
  }, { once: true })
}
function leave(el) {
  el.style.height = el.scrollHeight + 'px'
  requestAnimationFrame(() => {
    el.style.transition = 'height .25s ease'
    el.style.height = '0'
  })
}
</script>

<style scoped>
.collapse-enter-from,
.collapse-leave-to {
  opacity: 0;
}
.collapse-enter-active,
.collapse-leave-active {
  transition: opacity .25s ease;
}
</style>

<UCollapse>
  <template #label>Настройки</template>

  <div class="space-y-2">
    Контент, кнопки, формы — всё что нужно.
  </div>
</UCollapse>

<UCollapse variant="card" shadow>
  <template #label>Настройки аккаунта</template>
  <UInput placeholder="Email" />
</UCollapse>

<UCollapse variant="ghost" :rounded="false">
  <template #label>Фильтры</template>
  Содержимое...
</UCollapse>
