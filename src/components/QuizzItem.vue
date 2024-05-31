<template>
  <div class="hover:bg-gray-50 p-5 mb-5">
    <h1 class="text-4xl font-bold mb-5">
      {{ quizz.id }}. Hisoblang
      <span class="font-thin block my-10 mx-10">
        {{ quizz.question }}
      </span>
    </h1>
    <div class="flex flex-col">
      <label
        v-for="(answer, answerIndex) in quizz.answers"
        :key="answerIndex"
        :class="[
          'p-8 text-2xl',
          quizz.userAnswer === answer
            ? 'bg-slate-900 text-white'
            : 'hover:bg-primary',
        ]"
        :for="`id-${quizz.id}-${answerIndex}`"
      >
        {{ "abcd"[answerIndex] }}) {{ answer }}
        <input
          @change="selectAnswer(answerIndex)"
          class="hidden"
          type="radio"
          :name="`answer-${quizz.id}`"
          :id="`id-${quizz.id}-${answerIndex}`"
        />
      </label>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const props = defineProps({
  quizz: {
    type: Object,
    required: true,
  },
});

const userAnswer = ref(props.quizz.userAnswer);
const emit = defineEmits(["user-select-answer"]);

const selectAnswer = (answerIndex) => {
  const selectedAnswer = props.quizz.answers[answerIndex];
  userAnswer.value = selectedAnswer;
  emit("user-select-answer", {
    id: props.quizz.id,
    userAnswer: userAnswer.value,
  });
};
</script>
