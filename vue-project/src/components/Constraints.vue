<template>
  <div class="bg-white rounded-xl shadow-sm p-5 flex flex-col gap-1">

    <p class="text-xs font-bold uppercase tracking-wider text-slate-400">Course</p>
    <p class="text-sm font-semibold text-slate-800 mb-2">{{ courseName }}</p>

    <p class="text-xs font-bold uppercase tracking-wider text-slate-400">Instructor</p>
    <p class="text-sm font-semibold text-slate-800 mb-2">{{ instructor }}</p>

    <p class="text-xs font-bold uppercase tracking-wider text-slate-400">Progress</p>
    <div class="flex items-center gap-3 mt-1">
      <div class="flex-1 h-2 bg-slate-200 rounded-full overflow-hidden">
        <div
          class="h-full bg-gradient-to-r from-indigo-600 to-indigo-400 rounded-full transition-all duration-500"
          :style="{ width: progress + '%' }"
        ></div>
      </div>
      <span class="text-xs font-bold text-indigo-600">{{ progress }}%</span>
    </div>

  </div>
</template>

<script setup>
defineProps({
  courseName: { type: String, required: true },
  instructor: { type: String, default: 'TBA' },
  progress: {
    type: Number,
    default: 0,
    validator: (v) => v >= 0 && v <= 100,
  },
})
</script>