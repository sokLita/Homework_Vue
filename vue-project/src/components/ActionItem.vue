<template>
  <div
    class="bg-white rounded-2xl shadow-md hover:shadow-lg transition p-5 flex items-center gap-4 border border-slate-100"
  >
    <!-- Icon -->
    <div
      class="w-11 h-11 flex items-center justify-center rounded-xl text-xl shrink-0 transition"
      :class="{
        'bg-blue-100': type === 'read',
        'bg-yellow-100': type === 'watch',
        'bg-orange-100': type === 'build',
        'bg-green-100': type === 'submit',
      }"
    >
      {{ typeIcon }}
    </div>

    <!-- Body -->
    <div class="flex-1 min-w-0">
      <p class="font-semibold text-base text-slate-800 truncate">
        {{ task }}
      </p>

      <a
        v-if="link"
        :href="link"
        target="_blank"
        rel="noopener"
        class="text-sm text-indigo-600 hover:text-indigo-800 hover:underline truncate block mt-1"
      >
        {{ link }}
      </a>
    </div>

    <!-- Button -->
    <button
      class="shrink-0 bg-indigo-50 text-indigo-600 hover:bg-indigo-600 hover:text-white text-xs font-semibold px-4 py-2 rounded-lg transition active:scale-95"
      @click="$emit('done', task)"
    >
      Mark Done
    </button>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  task: { type: String, required: true },
  type: {
    type: String,
    default: 'read',
    validator: (v) => ['read', 'watch', 'build', 'submit'].includes(v),
  },
  link: { type: String, default: null },
})

defineEmits(['done'])

const typeIcon = computed(() => ({
  read: '',
  watch: '',
  build: '',
  submit: '',
}[props.type] ?? ''))
</script>