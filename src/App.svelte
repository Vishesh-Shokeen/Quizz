<script>
    import Questions from "./lib/Questions.svelte";
    import sun from "/sun.svg";
    import moon from "/moon.svg";
    import logo from "/logo.svg";

    let mode = false;
    let score = 0;
    let totalQuestion = 5;


    const toggleMode = () => {
        mode = !mode;
        const appBody = document.querySelector("body");
        appBody.classList.toggle("darkMode");
    };
</script>

<main>
    <header>
        <img src={logo} alt="Logo">
        <h1>Score : {score} /{totalQuestion} </h1>
        <img on:click={toggleMode} src={mode ? sun : moon} alt="ToggleMode" />
    </header>
    <Questions bind:score/>
</main>

<style>
    main{
        border: 1px solid rgba(146, 145, 145, 0.764);
        width: min(100% , 1200px);
        margin: 0 auto;
    }
    header {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 1rem;
    }
</style>
