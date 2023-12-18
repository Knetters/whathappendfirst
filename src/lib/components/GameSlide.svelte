<script>
    export let data

    import { onMount } from 'svelte';

    let selectedEvents = [];
    let input = "";
    let correctAnswer = ""
    let roundValue = "0"

    function fillGame(){
        const shuffledEvents = [...data.events].sort(() => 0.5 - Math.random());
        selectedEvents = shuffledEvents.slice(0, 2);

        let yearA = selectedEvents[0].year
        let yearB = selectedEvents[1].year

        // Calculate the absolute differences between the years and 0
        let differenceA = Math.abs(yearA - 0);
        let differenceB = Math.abs(yearB - 0);

        // Check which year is closer to 0
        if (differenceA < differenceB) {
            correctAnswer = "A"
        } else {
            correctAnswer = "B"
        }
    }

    function checkAnswer() {
        if (input === correctAnswer) {
            roundValue++;
            fillGame()
        } else {
            gameOver()
        }
    }

    function gameOver() {
        const GOUI = document.getElementById('game-over');
        if (GOUI.classList.contains('active')) {
            GOUI.classList.toggle('active');
            resetScore();
        } else {
            GOUI.classList.toggle('active');
        }
    }

    function resetScore() {
        roundValue = "0"
    }

    fillGame()

    onMount(() => {
        document.getElementById('a').addEventListener('click', function(event) {
            input = "A"
            checkAnswer() 
        });

        document.getElementById('b').addEventListener('click', function(event) {
            input = "B"
            checkAnswer() 
        });

        document.getElementById('playAgain').addEventListener('click', function(event) {
            gameOver()
            fillGame()
        });

        document.getElementById('play').addEventListener('click', function(event) {
            const startUI = document.getElementById("start")
            startUI.style.transform = "translateY(-100vh)";
        });

        
    });
</script>

<main>

    <div id="start" class="start">
        <h1>What happend first</h1>
        <p>Choose the event that happend first in history and build a streak!</p>
        <button id="play">Play</button>
    </div>

    <section class="round" id="1">
        <article id="a">
            <div class="image-container">
                <img src="{selectedEvents[0].image.url}" alt="">
            </div>
            <h2>{selectedEvents[0].title}</h2>
        </article>
        <article id="b">
            <div class="image-container">
                <img src="{selectedEvents[1].image.url}" alt="">
            </div>
            <h2>{selectedEvents[1].title}</h2>
        </article>
    </section>

    <h3>Streak: <span class="counter">{roundValue}</span></h3>

    <div id="game-over" class="game-over">
        <p class="game-over-text">Game Over</p>
        <p>You got <span class="counter">{roundValue}</span> right!</p>
        <button id="playAgain">Play again</button>
    </div>
</main>

<style>
    main {
        height: 100vh;
        position: relative;
        overflow: hidden;
    }

    h1 {
        font-family: 'Silkscreen', sans-serif;
        text-shadow: 3px 0px 0px #cb4f4f;
        margin-top: 20vh;
        margin-bottom: 1rem;
        font-size: 5rem;
        line-height: 5rem;
    }

    button {
        font-family: 'Silkscreen', sans-serif;
        padding: .8rem 3rem 1rem 3rem;
        font-size: 3rem;
        margin-top: 4rem;
        border: 3px solid #4869ff;
        background-color: #f2f2f2;
        cursor: pointer;
    }

    button:hover {
        opacity: .9;
    }

    h3 {
        position: absolute;
        bottom: 0;
        right: 0;
        margin: 3rem;
        color: #f2f2f2;
        background-color: #00000063;
        text-shadow: 0px 0px 10px #000;
        padding: .3rem 1rem;
        border-radius: .5rem;
        border: 3px solid #cbb24f;
    }

    .counter {
        font-family: 'Silkscreen', sans-serif;
    }

    section {
        width: 100vw;
        height: 100vh;
        display: flex;
        gap: 2rem;
        padding: 2rem;
        background-color: #000000;
    }

    article {
        position: relative;
        transition: .2s;
        cursor: pointer;
        width: 100%;
        opacity: .9;
    }

    article:hover {
        margin: -.2rem;
        opacity: 1;
    }

    .image-container {
        position: relative;
        height: 100%;
        width: 100%;
    }

    img {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    h2 {
        position: absolute;
        font-size: 1.5rem;
        color: #f2f2f2;
        background-color: #00000063;
        text-shadow: 0px 0px 10px #4d4d4d;
        padding: .3rem 1rem;
        top: 2rem;
        left: 50%;
        width: max-content;
        max-width: 90%;
        margin-left: auto;
        transform: translate(-50%, -50%);
        border-radius: .5rem;
        text-align: center;
        font-family: 'Silkscreen', sans-serif;
    }

    .start {
        position: absolute;
        top: 0;
        text-align: center;
        width: 100%;
        height: 100%;
        background-color: #000;
        color: #f2f2f2;
        padding: 2rem;
        z-index: 9999;
        transition: .2s ease-in-out;
    }

    .game-over {
        position: absolute;
        top: 0;
        text-align: center;
        width: 100%;
        height: 100%;
        background-color: #000;
        color: #f2f2f2;
        padding: 2rem;
        margin-top: 100vh;
        transition: .2s ease-in-out;
    }

    .game-over span {
        color: #cbb24f;
    }

    .game-over-text {
        font-family: 'Silkscreen', sans-serif;
        text-shadow: 3px 0px 0px #cb4f4f;
        margin-top: 20vh;
        font-size: 5rem;
    }
</style>
  