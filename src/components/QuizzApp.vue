<template>
  <div>
    <div
      :class="isSideMenu ? 'w-[calc(100%-45%)]' : 'w-[calc(100%-65px)]'"
      class="shadow-md duration-300"
    >
      <h1 class="py-8 text-3xl text-center">Matematika</h1>
    </div>

    <div
      class="m-10 duration-300"
      :class="isSideMenu ? 'w-[calc(100%-50%)] ' : 'w-[1000px] mx-auto '"
    >
      <QuizzItem
        v-for="(quizz, index) in quizzes"
        :key="index"
        :quizz="quizz"
        @user-select-answer="handleSelectAnswer"
        @user-remove-answer="handleRemoveAnswer"
      />
    </div>

    <div
      :class="isSideMenu ? 'w-[calc(100%-45%)]' : 'w-[calc(100%-65px)]'"
      class="bg-black flex duration-300 py-40 justify-center items-center flex-wrap gap-6"
    >
      <div v-if="isFinishTest" class="text-white w-full text-xs">
        <div class="absolute ml-[750px] mt-[90px]">
          <h1 class="text-center">To'g'ri javoblar soni : {{ trueQuizzes }}</h1>
          <h1 class="text-center">Umumiy foiz : {{ percentQuizzes }} %</h1>
          <h1 class="text-center">Umumiy ball : {{ userBall }} / 100</h1>
        </div>

        <circle-progress :percent="percentQuizzes" class="mx-auto mt-6" />
      </div>

      <button
        @click="isFinishTest ? finishTestExit() : finishTest()"
        class="py-3 px-6 rounded-md bg-green-600 text-xl text-white"
      >
        {{ isFinishTest ? "Qayta topshirish" : "Testni yakunlash" }}
      </button>
    </div>

    <QuizzSider
      :quizzes="quizzes"
      v-model="isSideMenu"
      @finish:test="finishTestExit"
    />
  </div>
</template>

<script setup>
import "vue3-circle-progress/dist/circle-progress.css";
import CircleProgress from "vue3-circle-progress";
import { ref, reactive } from "vue";
import QuizzSider from "./QuizzSider.vue";
import QuizzItem from "./QuizzItem.vue";

const isSideMenu = ref(false);

const quizzes = reactive([
  {
    id: 1,
    question: "7*7 = ?",
    answers: ["40", "22", "49", "14"],
    correctAnswer: "49",
  },
  {
    id: 2,
    question: "100-50 = ?",
    answers: ["60", "20", "50", "1"],
    correctAnswer: "50",
  },
  {
    id: 3,
    question: "9/3 = ?",
    answers: ["8", "2", "3", "5"],
    correctAnswer: "3",
  },
  {
    id: 4,
    question: "5*(2+2) = ?",
    answers: ["4", "20", "3", "7"],
    correctAnswer: "20",
  },
  {
    id: 5,
    question: "1-1 = ?",
    answers: ["0", "2", "0", "5"],
    correctAnswer: "0",
  },
  {
    id: 6,
    question: "8*2 = ?",
    answers: ["18", "12", "58", "16"],
    correctAnswer: "16",
  },
  {
    id: 7,
    question: "2+10 = ?",
    answers: ["4", "2", "12", "5"],
    correctAnswer: "12",
  },
  {
    id: 8,
    question: "9+1 = ?",
    answers: ["4", "16", "10", "8"],
    correctAnswer: "10",
  },
  {
    id: 9,
    question: "77-7 = ?",
    answers: ["40", "83", "70", "5"],
    correctAnswer: "70",
  },
  {
    id: 10,
    question: "4*8 = ?",
    answers: ["32", "23", "35", "40"],
    correctAnswer: "32",
  },
]);

const handleSelectAnswer = (obj) => {
  quizzes.forEach((item) => {
    if (item.id === obj.id) {
      console.log("object add");
      item.userAnswer = obj.userAnswer;
    }
  });
};

const handleRemoveAnswer = (obj) => {
  quizzes.forEach((item) => {
    console.log("object remove");
    if (item.id === obj.id) {
      delete item.userAnswer;
    }
  });
};

const isFinishTest = ref(false);
const trueQuizzes = ref(0);
const percentQuizzes = ref(0);
const userBall = ref(0);

const finishTest = () => {
  isFinishTest.value = true;
  trueQuizzes.value = quizzes.reduce(
    (acc, item) => acc + (item.correctAnswer === item.userAnswer ? 1 : 0),
    0
  );
  percentQuizzes.value = (trueQuizzes.value * 100) / quizzes.length;
  userBall.value = percentQuizzes.value;
};
</script>

<style lang="scss" scoped></style>
