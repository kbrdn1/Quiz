<template>
  <div class="flex w-full flex-col gap-5">
    <h3>{{ question.question }}</h3>
    <ul>
      <li class="list-none" v-for="(choice, index) in question.options" :key="choice">
        <label class="flex items-center gap-2" :for="`answer${index}`">
          <input :id="`answer${index}`" type="radio" name="answer" v-model="answer" :disabled="buttonVerify === true" :value="choice" />
          <p>{{ choice }}</p>
        </label>
      </li>
    </ul>
    <span class="text-red-500" v-if="isWrongAnswer">Mauvaise réponse !</span>
    <span class="text-green-500" v-if="isRightAnswer">Bonne réponse !</span>
    <button @click="correctAnswer(answer)" v-if="!buttonVerify" class="p-3 bottom-0 right-0"
      :disabled="!hasAnswer">
      Vérifier
    </button>
    <button :disabled="!hasAnswer" @click="emit('answer', answer)" v-if="buttonVerify"
      class="p-3 bottom-0 right-0">
      Question suivante
    </button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  question: Object
})

const answer = ref(null)
const emit = defineEmits(['answer'])
const buttonVerify = ref(false)
const isWrongAnswer = ref(false) 
const isRightAnswer = ref(false) 

const hasAnswer = computed(() => answer.value !== null)

const correctAnswer = (choice) => {
  buttonVerify.value = true
  if (choice !== props.question.answer) {
    isWrongAnswer.value = true 
  } else {
    isWrongAnswer.value = false 
  }
  if (choice === props.question.answer) {
    isRightAnswer.value = true 
  } else {
    isRightAnswer.value = false 
  }
}

</script>
