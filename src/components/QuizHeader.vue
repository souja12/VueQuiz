<script setup>
    import {defineProps} from "vue"

    const {
        questionStatus,
        barPercentage
    } = defineProps([
        "questionStatus",
        "barPercentage"
    ])
    
</script>

<template>
    <header>
        <h4>Question {{ questionStatus === '4/3' ? '3/3' : questionStatus }}</h4>
    </header>
</template>

<style scoped>
    header {
        margin-top: 20px
    }

    header h4 {
        font-size: 30px;
    }

    .bar {
        width: 300px;
        height: 50px;
        border: 3px solid bisque 
    }

    .completion {
        height: 100%;
        width: 0%;
        background-color: bisque;
    }

</style>