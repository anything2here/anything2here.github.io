<template>
  <div class="p-4 border rounded shadow-md w-full max-w-md mx-auto">
    <p class="text-lg font-semibold mb-4">{{ question.question }}</p>
    <div class="flex flex-col gap-2">
      <button @click="$emit('answer', question.option_a.value)"
              class="px-4 py-2 bg-blue-500 text-white rounded">{{ question.option_a.text }}</button>
      <button @click="$emit('answer', question.option_b.value)"
              class="px-4 py-2 bg-green-500 text-white rounded">{{ question.option_b.text }}</button>
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps({
  question: Object
})
</script>