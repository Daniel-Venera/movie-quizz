<template>
    <div id="app">
        <Header :numCorrect="numCorrect" :numTotal="numTotal" />
        <b-container class="bv-example-row">
            <b-row>
                <b-col md="6" offset-md="3">
                    <b-jumbotron v-if="!began">
                        <p>
                            You are facing a hard movie quizz !
                        </p>
                        <b-button variant="primary" @click="begin">Begin</b-button>
                    </b-jumbotron>
                    <QuestionBox v-if="questions.length && began" :questionIndex="questionIndex" :currentQuestion="questions[questionIndex]" :next="next" :decoder="decoder" :increment="increment" :numCorrect="numCorrect" :numTotal="numTotal" />
                </b-col>
            </b-row>
        </b-container>
    </div>
</template>
<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
export default {
    name: "App",
    components: {
        Header,
        QuestionBox
    },
    data() {
        return {
            questions: [],
            questionIndex: 0,
            numCorrect: 0,
            numTotal: 0,
            began: false
        };
    },
    methods: {
        begin() {
            this.began = true;
        },
        next() {
            this.questionIndex++;
        },
        increment(isCorrect) {
            if (isCorrect) {
                this.numCorrect++;
            }
            this.numTotal++;
        },
        decoder(str) {
            var textArea = document.createElement("textarea");
            textArea.innerHTML = str;
            return textArea.value;
        }
    },
    mounted: function() {
        fetch("https://opentdb.com/api.php?amount=10&category=11&difficulty=hard&type=multiple")
            .then(response => {
                return response.json();
            })
            .then(jsonData => {
                this.questions = jsonData.results;
            });
    }
};
</script>
<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
