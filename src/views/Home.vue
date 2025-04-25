<template>
    <div class="min-h-screen flex flex-col items-center justify-center p-4">
      <div class="flex justify-between items-center w-full max-w-md mb-4">
        <h1 class="text-xl font-bold">{{ $t('title') }}</h1>
        <button @click="switchLanguage" class="text-sm underline">
          {{ $t('lang_switch') }}
        </button>
      </div>
  
      <QuestionCard
        v-if="currentQuestion"
        :question="currentQuestion"
        @answer="handleAnswer"
      />
  
      <div v-else class="w-full max-w-md bg-white rounded-2xl shadow-md p-6 mt-6">
        <p class="text-center text-xl font-bold mb-2">
        {{ $t('result') }} {{ resultType }}
        </p>
        <p class="text-sm text-gray-700 text-center mb-4">
        {{ explanation }}
        </p>
        <div class="flex justify-center">
          <button @click="restart" class="px-4 py-2 bg-gray-700 text-white rounded-xl shadow">
            {{ $t('restart') }}
          </button>
        </div>
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
  const explanation = computed(() => locale.value === 'zh' ? explanationZh.value : explanationEn.value)
  
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
  
  const typeDescriptionsEn: Record<string, string> = {
    ISTJ: "Responsible and reserved, ISTJs value tradition, rules, and loyalty. They are practical and thorough.",
    ISFJ: "Warm and dedicated, ISFJs are protectors and caretakers, focused on serving others and maintaining harmony.",
    INFJ: "Idealistic and compassionate, INFJs are insightful thinkers driven by values and a desire to help others.",
    INTJ: "Strategic and analytical, INTJs are independent problem-solvers with a long-term vision and deep focus.",
    ISTP: "Practical and action-oriented, ISTPs are hands-on problem solvers who enjoy understanding how things work.",
    ISFP: "Gentle and spontaneous, ISFPs seek beauty and harmony, and prefer to express themselves through actions.",
    INFP: "Creative and empathetic, INFPs are guided by their values and passionate about helping people and causes.",
    INTP: "Analytical and intellectual, INTPs enjoy abstract thinking and are constantly seeking to understand complex ideas.",
    ESTP: "Energetic and adaptable, ESTPs are spontaneous, love excitement, and live in the moment.",
    ESFP: "Fun-loving and enthusiastic, ESFPs are entertainers who enjoy life and bring energy to social interactions.",
    ENFP: "Imaginative and energetic, ENFPs are enthusiastic about new ideas, people, and possibilities.",
    ENTP: "Inventive and curious, ENTPs are natural debaters who thrive on exploring ideas and challenging norms.",
    ESTJ: "Organized and direct, ESTJs are efficient leaders who value tradition and getting things done.",
    ESFJ: "Caring and social, ESFJs are community-minded and driven by the desire to help and support others.",
    ENFJ: "Charismatic and altruistic, ENFJs are natural leaders who inspire others and seek to make a positive difference.",
    ENTJ: "Confident and strategic, ENTJs are decisive leaders who excel at planning and executing complex goals."
  }
  
  const typeDescriptionsZh: Record<string, string> = {
    ISTJ: "负责任、安静内敛，注重传统和规则，做事严谨，值得信赖。",
    ISFJ: "温和细心，富有责任感，关心他人，乐于维持和谐与秩序。",
    INFJ: "理想主义者，有深度思考能力，富有同理心，渴望助人改变世界。",
    INTJ: "有远见和战略眼光，追求逻辑和效率，善于独立规划和思考。",
    ISTP: "动手能力强，喜欢独立探索与实践，擅长分析和解决实际问题。",
    ISFP: "温柔、随和，喜欢用行动表达情感，欣赏美和自由的生活节奏。",
    INFP: "理想主义者，重视内在价值观，善于共情，渴望实现意义感目标。",
    INTP: "逻辑严密，思想深邃，热衷探索复杂概念，享受独立思考。",
    ESTP: "充满活力、果断直接，喜欢迎接挑战，擅长临场应变与行动。",
    ESFP: "开朗、热情，活在当下，善于带动气氛，喜欢与人互动与分享。",
    ENFP: "富有想象力与感染力，对人和世界充满好奇，追求自由和可能性。",
    ENTP: "思维敏捷、擅长辩论，热衷尝试新事物，喜欢挑战常规。",
    ESTJ: "务实高效、注重规则，是组织者和执行者，善于管理与落实计划。",
    ESFJ: "热情有爱，关心他人，擅长协调关系，是乐于服务的守护者。",
    ENFJ: "有领导魅力，富有同理心，关心集体，善于激励和帮助他人。",
    ENTJ: "自信果断，有远见和规划能力，擅长领导和制定宏观战略。"
  }
  
  const explanationEn = computed(() => typeDescriptionsEn[resultType.value] || '')
  const explanationZh = computed(() => typeDescriptionsZh[resultType.value] || '')
  </script>
  