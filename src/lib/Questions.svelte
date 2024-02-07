<script>
    import QuizDialog from "../lib/QuizDialog.svelte";
    export let score;
    export let totalQuestion;

    let questionNumber = 0;
    let url = `https://opentdb.com/api.php?amount=5&category=9&difficulty=medium&type=multiple`;
    $: data = fetchApi(url);

    const fetchApi = async (url) => {
        const res = await fetch(url);
        const data = await res.json();
        return data;
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

    const restartQuizFn = () => {
        score = 0;
        questionNumber = 0;
        data = fetchApi(url)
    };

    const checkAnswerFn = (value, correct_answer) => {
        const optionSelected = document.getElementById(`${value}`);
        const correctOption = document.getElementById(`${correct_answer}`);
        const optionsWrapper = document.querySelector("#optionsWrapper");

        optionsWrapper.setAttribute("disable", true);
        correctOption.setAttribute("correct", true);
        optionSelected === correctOption
            ? ++score
            : optionSelected.setAttribute("incorrect", true);
    };

    const startQuizFn = () => {
        const dialog = document.querySelector("#quizDialog");
        dialog.showModal();
    };
</script>


{#await data}
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


    <div id="btnWrapper">
        <button id="setParamBtn" on:click={startQuizFn}>New Params</button>
        <button id="restartBtn" on:click={restartQuizFn}>Restart</button>
        <button
        id="nextBtn"
        on:click={() => nextQuestionFn(data.results.length - 1)}
        >Next Question</button
        >
    </div>
{:catch}
    <h3>Something rong</h3>
{/await}

<QuizDialog bind:url  bind:totalQuestion/>
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

    #btnWrapper {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 1rem;
        margin: 1rem 0;
    }

    #nextBtn:hover , #restartBtn:hover ,#setParamBtn:hover  {
        background-color:rgba(185, 183, 183, 0.756);
    }
</style>
