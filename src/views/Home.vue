<template>
  <div class="min-h-screen flex flex-col items-center justify-center p-4">
    <div class="flex justify-between items-center w-full max-w-md mb-4">
      <h1 class="text-xl font-bold">{{ $t('title') }}</h1>
      <button @click="switchLanguage" class="text-sm underline">{{ $t('lang_switch') }}</button>
    </div>
    <QuestionCard v-if="currentQuestion" :question="currentQuestion" @answer="handleAnswer" />
    <div v-else class="text-center mt-6">
      <p class="mb-2">{{ $t('result') }} {{ resultType }}</p>
      <p class="mb-4">{{ $t('explanation') }} {{ explanationMap[resultType] || '-' }}</p>
      <button @click="restart" class="px-4 py-2 bg-gray-600 text-white rounded">{{ $t('restart') }}</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import { useI18n } from 'vue-i18n'
import QuestionCard from '../components/QuestionCard.vue'
import questionsZh from '../data/questions.zh.json'
import questionsEn from '../data/questions.en.json'

const { locale } = useI18n()
const index = ref(0)
const answers = ref<string[]>([])

const questions = computed(() => locale.value === 'zh' ? questionsZh : questionsEn)
const currentQuestion = computed(() => questions.value[index.value])

const handleAnswer = (value: string) => {
  answers.value.push(value)
  index.value++
}

const restart = () => {
  index.value = 0
  answers.value = []
}

const switchLanguage = () => {
  locale.value = locale.value === 'zh' ? 'en' : 'zh'
}

const resultType = computed(() => {
  const resultMap: Record<string, number> = { E: 0, I: 0, S: 0, N: 0, T: 0, F: 0, J: 0, P: 0 }
  answers.value.forEach(val => resultMap[val]++)
  return `${resultMap.E >= resultMap.I ? 'E' : 'I'}${resultMap.S >= resultMap.N ? 'S' : 'N'}${resultMap.T >= resultMap.F ? 'T' : 'F'}${resultMap.J >= resultMap.P ? 'J' : 'P'}`
})

const explanationMap = {
  ENFP: '充满激情与创造力，热爱探索世界。',
  ISTJ: '务实可靠，注重细节与计划。',
  // 可补充更多类型说明
}
</script>