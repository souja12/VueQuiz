<script setup>
    import {defineProps} from "vue"
    import {RouterLink} from "vue-router"
    import Cookies from 'js-cookie'; 

    const {numberOfCorrectAnswers, quizQuestionLength, quizId} = defineProps(["quizQuestionLength", "numberOfCorrectAnswers", "quizId"])


    const resetCookieStorage = () => {
        const cookieKey = `lastAnsweredQuestionIndex_${quizId}`;
        Cookies.set(cookieKey, '0', { expires: 365 });
    };
</script>

<template>
    <div class="results">
        <p>Your Results...</p>
        <h1>
            {{numberOfCorrectAnswers}}/{{quizQuestionLength}}
        </h1>
        <RouterLink to="/" @click="resetCookieStorage">Go Back</RouterLink>
    </div>
</template>

<style scoped>
    .results {
        text-align: center;
        padding: 100px 0;
    }

    p {
        font-size: 25px;
    }

    h1 {
        font-size: 80px;
    }
</style>