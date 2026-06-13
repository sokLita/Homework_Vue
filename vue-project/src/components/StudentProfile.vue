<template>
  <div class="bg-white rounded-xl shadow-sm p-5">

    <!-- Header -->
    <div class="flex items-center gap-4 mb-5">
      <img
        v-if="avatar"
        :src="avatar"
        :alt="`${name} avatar`"
        class="w-12 h-12 rounded-full object-cover shrink-0"
      />
      <div
        v-else
        class="w-12 h-12 rounded-full bg-indigo-600 text-white font-extrabold text-sm flex items-center justify-center shrink-0"
      >
        {{ name.charAt(0) }}
      </div>
      <div>
        <p class="font-bold text-slate-800 text-sm">{{ name }}</p>
        <p class="text-xs text-slate-400 mt-0.5">{{ bio }}</p>
      </div>
    </div>

    <!-- Plans -->
    <div v-if="plans && plans.length">
      <p class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-3">
        Learning Plans
      </p>
      <ul class="flex flex-col gap-2">
        <li
          v-for="(plan, idx) in plans"
          :key="idx"
          class="flex items-center gap-2 text-sm text-slate-700"
        >
          <span class="w-2 h-2 rounded-full bg-indigo-500 shrink-0"></span>
          {{ plan }}
        </li>
      </ul>
    </div>

  </div>
</template>

<script setup>
defineProps({
  avatar: { type: String},
  name:   { type: String, required: true },
  bio:    { type: String, default: '' },
  plans:  { type: Array,  default: () => [] },
})
</script>
