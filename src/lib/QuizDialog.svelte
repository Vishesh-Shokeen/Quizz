<script>
    export let url;
    export let totalQuestion;
    let category_id = '9';
    let num = '5';
    const categories = {
        'General Knowledge': 9,
        "Books": 10,
        "Film": 11,
        "Music": 12,
        "Video Games": 15,
        "Science & Nature": 17,
        "Computers": 18,
        "Mathematics": 19,
        "Sports": 21,
        "Geography": 22,
        'History': 23,
        'Art': 25,
        'Celebrities': 26,
        'Animals': 27,
        'Vehicles': 28
    }

    const keys = Object.keys(categories)
    const startQuizFn = () => {
        totalQuestion = num
        const dialog = document.querySelector('#quizDialog')
        dialog.close()
        url = `https://opentdb.com/api.php?amount=${num}&category=${category_id}&difficulty=medium&type=multiple`
    }
</script>

<dialog id="quizDialog">
    <div>
        <p>Select-Category</p>
        <select bind:value={category_id}>
            {#each keys as category}
            <option value={categories[category]}>{category}</option>
            {/each}
        </select>
        <p>Number of questions</p>
        <select bind:value={num}>
            <option value="5">5</option>
            <option value="10">10</option>
            <option value="20">20</option>
        </select>
        <button on:click={startQuizFn}>Start</button>
    </div>
</dialog>