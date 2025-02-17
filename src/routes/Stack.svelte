<script lang="ts">
    export let size = 6;
    export let name = "";
    export let cornerModifier = true;
    export let excludedColor = "none";
    export let scores: number[] = [];

    let cornerModifiers = ["", "+", "-"];
    let cornerModifierIndex = 0;

    let colors = ["red", "none", "blue"];
    let rings = Array(size).fill("default");

    let currentCornerModifier = "";

    let currentIndex = size;

    function addSquare(color: string) {
        if (currentIndex != 0) {
            currentIndex--;
            rings[currentIndex] = color;
        }
        calculateScore();
    }

    function removeSquare() {
        if (currentIndex != size) {
            rings[currentIndex] = "default";
            currentIndex++;
        }
        calculateScore();
    }

    function cycleCornerModifier() {
        if (!cornerModifier) {
            return;
        }
        cornerModifierIndex = (cornerModifierIndex + 1) % 3;
        currentCornerModifier = cornerModifiers[cornerModifierIndex];
        calculateScore();
    }

    function calculateScore() {
        let redScore = 0;
        let blueScore = 0;

        let j;
        for (j = 0; j < size; j++) {
            if (rings[j] !== "default") {
                break;
            }
        }

        for (let i = j; i < size; i++) {
            let scoreScalar = i === j ? 3 : 1;
            if (rings[i] === "red") {
                redScore += scoreScalar;
            } else if (rings[i] === "blue") {
                blueScore += scoreScalar;
            }
        }

        if (currentCornerModifier === "+") {
            redScore *= 2;
            blueScore *= 2;
        } else if (currentCornerModifier === "-") {
            redScore *= -1;
            blueScore *= -1;
        }

        scores = [redScore, blueScore];
    }
</script>

<div class="name-display">
    {name}
    {#if cornerModifier}
    <span
        class={cornerModifierIndex === 0
            ? "tilde-btn"
            : cornerModifierIndex === 1
              ? "plus-btn"
              : "minus-btn"}
        style="width: 60px !important;"
    ></span>
    {/if}
</div>

<div class="container">
    <button class="stack-wrapper" on:click={() => cycleCornerModifier()}>
        <div class="stack">
            {#each rings as color, i}
                <div
                    class="ring"
                    style="background-color: {color === 'red'
                        ? 'red'
                        : color === 'blue'
                          ? 'blue'
                          : 'white'};"
                ></div>
            {/each}
        </div>
    </button>
</div>

<div class="bottom-controls">
    {#each colors as color, _}
        {#if color !== excludedColor && color !== "none"}
            <button
                class="{color}-btn"
                on:click={() => addSquare(color)}
                aria-label="add {color} ring"
                style="{excludedColor !== "none" ? "width: 50% !important;" : ""}"
            ></button>
        {/if}
        {#if color === "none"}
            <button
                class="minus-btn"
                on:click={() => {
                    removeSquare();
                }}
                aria-label="remove ring"
                style="{excludedColor !== "none" ? "width: 50% !important;" : ""}"
            ></button>
        {/if}
    {/each}
</div>

<style>
    @import "./buttons.css";

    .name-display {
        display: flex;
        justify-content: center;
        gap: 30px;
    }

    .container {
        display: flex;
        align-items: center;
        justify-content: center;
        width: fit-content;
        margin: auto;
        padding: 10px;
    }

    .stack-wrapper {
        display: flex;
        flex-direction: column;
        align-items: center;
        all: unset;
    }

    .stack {
        display: flex;
        flex-direction: column;
        gap: 10px;
        align-items: center;
    }

    .ring {
        width: 300px;
        height: 20px;
        border: none;
        display: flex;
        justify-content: center;
        align-items: center;
        border-radius: 5px;
    }

    .side-controls {
        display: flex;
        flex-direction: column;
        gap: 10px;
        margin-left: 20px;
        align-items: center;
        justify-content: center;
        height: 100%;
    }

    .bottom-controls {
        display: flex;
        justify-content: center;
        gap: 10px;
        margin-top: 20px;
    }
</style>
