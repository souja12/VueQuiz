<!-- <script setup>
    import Question from "../components/Question.vue"
    import QuizHeader from "../components/QuizHeader.vue"
    import Result from "../components/Result.vue"
    import {useRoute} from "vue-router"
    import { ref, computed } from "vue";
    import quizes from "../data/quizes.json"

    const route = useRoute()
    const quizId = parseInt(route.params.id);
    const quiz = quizes.find(q => q.id === quizId)
    const currentQuestionIndex = ref(0);
    const numberOfCorrectAnswers = ref(0)
    const showResults = ref(false)

    // const questionStatus = ref(`${currentQuestionIndex.value}/${quiz.questions.length}`)

    // watch(() => currentQuestionIndex.value, () => {
    //     questionStatus.value = `${currentQuestionIndex.value}/${quiz.questions.length}`
    // })

    const questionStatus = computed(() => `${currentQuestionIndex.value}/${quiz.questions.length}`)
    const barPercentage = computed(() => `${currentQuestionIndex.value/quiz.questions.length * 100}%`)

    const onOptionSelected = (isCorrect) => {
        if(isCorrect){
            numberOfCorrectAnswers.value++;
        }

        if(quiz.questions.length - 1 === currentQuestionIndex.value){
            showResults.value = true
        }

        currentQuestionIndex.value++;
    }
</script> -->

<script setup>
import { ref, computed, onMounted } from "vue";
import Question from "../components/Question.vue";
import QuizHeader from "../components/QuizHeader.vue";
import Result from "../components/Result.vue";
import { useRoute } from "vue-router";
import quizes from "../data/quizes.json";
import Cookies from "js-cookie";

const route = useRoute();
const quizId = parseInt(route.params.id);
const quiz = quizes.find((q) => q.id === quizId);
const currentQuestionIndex = ref(0);
const numberOfCorrectAnswers = ref(0);
const showResults = ref(false);

const questionStatus = computed(
  () => `${currentQuestionIndex.value + 1}/${quiz.questions.length}`
);


// Set the cookie key including the quiz ID
const setCookie = (quizId, index) => {
    const cookieKey = `lastAnsweredQuestionIndex_${quizId}`;
    Cookies.set(cookieKey, index.toString(), { expires: 365 }); // Expires in 1 year
};

// Read the cookie key including the quiz ID
const getCookie = (quizId) => {
    const cookieKey = `lastAnsweredQuestionIndex_${quizId}`;
    return Cookies.get(cookieKey);
};

const onOptionSelected = (isCorrect) => {
  if (isCorrect) {
    numberOfCorrectAnswers.value++;
  }

  if (quiz.questions.length -1 === currentQuestionIndex.value) {
    showResults.value = true;
  }

  currentQuestionIndex.value++;
  setCookie(quizId, currentQuestionIndex.value);
};

onMounted(() => {
  const lastAnsweredQuestionIndex = getCookie(quizId);
  if (lastAnsweredQuestionIndex !== undefined) {
    // Attempt to parse the string value back to an integer
    const parsedIndex = parseInt(lastAnsweredQuestionIndex);
    if (!isNaN(parsedIndex)) {
      currentQuestionIndex.value = parsedIndex;
    } else {
      // Handle case where parsing fails (e.g., invalid value stored in the cookie)
      console.error("Invalid value stored in cookie");
    }
  } else {
    // Handle case where cookie is not set
    console.error("Cookie 'lastAnsweredQuestionIndex' not found");
  }
});
</script>

<template>
  <div>
    <QuizHeader
      :questionStatus="questionStatus"
    />
    <div>
      <Question
        v-if="!showResults"
        :question="quiz.questions[currentQuestionIndex]"
        @selectOption="onOptionSelected"
      />
      <Result
        v-else
        :quizQuestionLength="quiz.questions.length"
        :numberOfCorrectAnswers="numberOfCorrectAnswers"
        :quizId="quizId"
      />
    </div>
  </div>
</template>
