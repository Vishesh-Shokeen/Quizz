<script>
    import { fade } from "svelte/transition";
    export let score;
    let dataArray;
    let questionNumber = 0;
    const url =
        "https://opentdb.com/api.php?amount=5&category=17&difficulty=easy&type=multiple";
    const fetchApi = async () => {
        const res = await fetch(url);
        const data = await res.json();
        return data;
    };

    dataArray = fetchApi();
    const restartQuiz = () => {
        score = 0;
        questionNumber = 0;
        dataArray = fetchApi();
    };

    const checkAnswerFn = (value, correct_answer) => {
        const optionSelected = document.getElementById(`${value}`);
        const correctOption = document.getElementById(`${correct_answer}`);
        const optionsWrapper = document.querySelector("#optionsWrapper");

        optionsWrapper.setAttribute("disable", true);

        correctOption.setAttribute("correct", true);
        if (optionSelected === correctOption) {
            ++score;
        } else {
            optionSelected.setAttribute("incorrect", true);
        }
    };

    const nextQuestionFn = (quizLength) => {
        const btn = document.querySelector("#nextBtn");
        const optionsWrapper = document.querySelector("#optionsWrapper");
        const options = document.querySelectorAll("span");
        optionsWrapper.removeAttribute("disable");
        options.forEach((option) => {
            option.removeAttribute("correct");
            option.removeAttribute("incorrect");
        });

        questionNumber < quizLength
            ? ++questionNumber
            : btn.setAttribute("disabled", true);
    };
</script>

{#await dataArray}
    <h3>Loading...</h3>
{:then data}
    <h3>{@html data.results[questionNumber].question}</h3>
    <div id="optionsWrapper">
        {#each [...data.results[questionNumber].incorrect_answers, data.results[questionNumber].correct_answer].sort() as option}
            <span
                id={option}
                on:click={() =>
                    checkAnswerFn(
                        option,
                        data.results[questionNumber].correct_answer,
                    )}>{@html option}</span
            >
        {/each}
    </div>
    <div id='btnWrapper'>

        <button
        disabled={questionNumber === data.results.length - 1}
        id="nextBtn"
        on:click={() => nextQuestionFn(data.results.length - 1)}
        >
        Next Question
    </button>
    <button id="restartBtn" on:click={restartQuiz}>New Quiz</button>
</div>
{:catch}
<h3>Network Error</h3>
{/await}

<style>
    h3 {
        text-align: center;
    }
    div {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 1rem;
    }

    span {
        border: 1px dashed;
        padding: 1rem;
        text-align: center;
        margin: 0.2rem;
    }
    span:hover{
        background-color: rgba(176, 173, 173, 0.814);
    }

    #btnWrapper{
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 1rem;
        margin:1rem 0;
    }
    #nextBtn:hover {
        background-color:rgb(172, 172, 244);;
    }
    #restartBtn:hover {
        background-color:rgb(66, 176, 73);
    }
</style>
