<script setup lang="ts">
import { ref, watch } from 'vue'
import QData from './data.json'
import { type Ref } from 'vue'
interface FeedbackMessages {
  [key: string]: string
}

let num: Ref<number> = ref(1)
let feedbackMessages: Ref<FeedbackMessages> = ref({})
let correct: Ref<number> = ref(0)

const next = () => {
  num.value++
  feedbackMessages.value = {}
}

const correctAnswer = (name: string) => {
  const correctOption = data.value?.answer
  feedbackMessages.value[name] = name === correctOption ? 'Correct!' : 'Wrong!'
  if (name !== correctOption) {
    feedbackMessages.value[correctOption!] = 'Correct answer!'
  }
  correct.value += name === correctOption ? 1 : 0
  setTimeout(next, 1000)
}

const data = ref(QData.find((id: any) => id.id == num.value))

watch(
  () => num.value,
  () => {
    data.value = QData.find((id: any) => id.id == num.value)
  }
)
</script>

<template>
  <main class="flex items-center justify-center w-screen bg-cover h-screen bg-[url(/bg.jpg)]">
    <div
      class="flex flex-col bg-[rgb(52,57,99)] gap-7 p-4 lg:px-32 lg:py-10 justify-center items-center mx-4 rounded-xl"
    >
      <p class="text-center justify-self-start">Country Quizz</p>
      <div class="flex w-full flex-wrap gap-4">
        <p
          v-for="number in QData.length"
          :key="number"
          :class="{
            'from-[#E65895] bg-gradient-to-r to-[#BC6BE8]': number <= num,
            'bg-[#393f6f]': number > num
          }"
          class="h-11 w-11 p-3 rounded-full text-center text-white"
        >
          {{ number }}
        </p>
      </div>
      <div v-if="num != QData.length">
        <h1 class="text-white text-xl text-center">
          {{ data?.question }}
        </h1>
        <ul class="grid grid-cols-1 md:grid-cols-2 text-center gap-4 p-4">
          <li class=" " v-for="option in data?.options">
            <button
              class="w-full h-full px-20 font-semibold rounded-lg py-4"
              :class="{
                'bg-[#393f6f] text-white': !feedbackMessages[option],
                'bg-green-500': feedbackMessages[option] === 'Correct!',
                'bg-red-500': feedbackMessages[option] === 'Wrong!'
              }"
              v-on:click="correctAnswer(option)"
            >
              {{ feedbackMessages[option] || option }}
            </button>
          </li>
        </ul>
      </div>
      <div v-if="num >= QData.length">
        <p>Congrats! You completed the quiz.</p>
        <p>{{ correct }} /{{ QData.length }}</p>
      </div>
    </div>
  </main>
</template>
>

<style>
@tailwind base;
@tailwind components;
@tailwind utilities;
#app {
  display: flex;
  max-width: fit-content;
  margin: auto;
  padding: 0;
}
</style>
