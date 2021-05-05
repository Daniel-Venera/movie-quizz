<template>
    <div class="questionBoxContainer">
        <b-jumbotron>
            <template slot="lead">
                {{ decoder(currentQuestion.question) }}
            </template>
            <hr class="my-4" />
            <b-list-group>
                <b-list-group-item v-for="(answer, index) in shuffledAnswers" :key="index" @click="selectAnswer(index)" :class="[selectedIndex == index && correctIndex !== index ? 'incorrect' : '', selectedIndex !== null && correctIndex == index ? 'correct' : '', selectedIndex !== null ? 'list-group-item--userSelected' : '']">{{ decoder(answer) }}</b-list-group-item>
            </b-list-group>
            <b-button variant="success" v-if="selectedIndex !== null && questionIndex !== 10" href="#" @click="next()">Next</b-button>
            <div class="result" v-if="questionIndex == 10">
                <p>Result : {{ numCorrect }}/{{ numTotal }}</p>
            </div>
        </b-jumbotron>
    </div>
</template>
<script>
import _ from "lodash";
export default {
    props: {
        currentQuestion: Object,
        next: Function,
        decoder: Function,
        increment: Function,
        numCorrect: Number,
        numTotal: Number
    },
    data() {
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            questionIndex: 0
        };
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers];
            answers.push(this.currentQuestion.correct_answer);
            return answers;
        }
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                this.selectedIndex = null;
                this.shuffleAnswers();
            }
        }
    },
    methods: {
        selectAnswer(index) {
            this.selectedIndex = index;
            let isCorrect = false;
            if (this.selectedIndex == this.correctIndex) {
                isCorrect = true;
            }
            this.increment(isCorrect);
            this.questionIndex++;
        },
        shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
            this.shuffledAnswers = _.shuffle(answers);
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
        }
    }
};
</script>
<style scoped>
.list-group {
    margin-bottom: 15px;
}
.list-group-item:hover {
    background-color: rgb(223, 223, 223);
    cursor: pointer;
}
.list-group-item--userSelected:hover {
    background-color: white;
    cursor: default;
}
.btn {
    margin: 0px 5px;
}
.selected {
    background-color: lightblue;
}
.incorrect,
.incorrect:hover {
    background-color: red;
}
.correct,
.correct:hover {
    background-color: lightgreen;
}
.result {
    margin-top: 48px;
    font-size: 28px;
    font-weight: 700;
}
</style>
