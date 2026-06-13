<template>
  <div
    class="bg-white rounded-xl shadow-sm p-4 flex items-center justify-between gap-3 border-l-4 transition hover:shadow-md"
    :class="status === 'completed' ? 'border-green-500 opacity-75' : 'border-amber-400'"
  >
    <!-- Info -->
   <div class="flex flex-col gap-1 min-w-0">
    <p class="font-bold text-slate-800 text-sm">{{ title }}</p>
    <a
        v-if="website"
        :href="resolvedLink"
        target="_blank"
        rel="noopener"
        class="text-indigo-600 text-xs hover:underline"
        >
        {{ website }} 
        </a>
        <span v-if="dueDate" class="text-xs text-slate-400">Due: {{ dueDate }}</span>
    </div>

    <!-- Actions -->
    <div class="flex items-center gap-2 shrink-0">
      <span
        class="text-xs font-bold px-3 py-1 rounded-full whitespace-nowrap"
        :class="status === 'completed'
          ? 'bg-green-100 text-green-700'
          : 'bg-amber-100 text-amber-700'"
      >
        {{ status === 'completed' ? ' Completed' : 'Pending' }}
      </span>
      <button
        v-if="status !== 'completed'"
        class="bg-indigo-600 hover:bg-indigo-700 active:scale-95 text-white text-xs font-bold px-3 py-1.5 rounded-lg transition"
        @click="$emit('submit', title)"
      >
        Submit
      </button>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  title:   { type: String, required: true },
  website: { type: String, default: null },
  status:  {
    type: String,
    default: 'pending',
    validator: (v) => ['pending', 'completed'].includes(v),
  },
  dueDate: { type: String, default: null },
})

defineEmits(['submit'])

const resolvedLink = computed(() => {
  if (!props.website) return '#'
  if (props.website.startsWith('http')) return props.website
  return 'https://vuejs.org'
})
</script>