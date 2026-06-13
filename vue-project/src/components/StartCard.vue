<template>
  <div
    class="bg-white rounded-xl shadow-sm p-5 flex flex-col gap-2 border-t-4"
    :class="{
      'border-indigo-500': status === 'good',
      'border-amber-400':  status === 'pending',
      'border-green-500':  status === 'excellent',
      'border-red-500':    status === 'danger',
    }"
  >
    <p class="text-xs font-semibold uppercase tracking-wider text-slate-400">
      {{ title }}
    </p>

    <div class="flex items-baseline gap-1">
      <span class="text-4xl font-extrabold text-slate-800 leading-none">{{ value }}</span>
      <span class="text-sm text-slate-400">{{ label }}</span>
    </div>

    <span
      class="self-start text-xs font-bold px-2 py-0.5 rounded-full"
      :class="{
        'bg-slate-100 text-slate-500':   status === 'good',
        'bg-amber-100 text-amber-700':   status === 'pending',
        'bg-green-100 text-green-700':   status === 'excellent',
        'bg-red-100   text-red-600':     status === 'danger',
      }"
    >
      {{ statusText }}
    </span>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  title:  { type: String, required: true },
  value:  { type: Number, required: true },
  label:  { type: String, default: '' },
  status: {
    type: String,
    default: 'good',
    validator: (v) => ['good', 'pending', 'excellent', 'danger'].includes(v),
  },
})

const statusText = computed(() => ({
  good:      'On Track',
  pending:   'Pending',
  excellent: 'Excellent',
  danger:    'Needs Work',
}[props.status]))
</script>