<script lang="ts">
    export let size = 6;
    export let cornerModifier = true;
    export let excludedColor = "none";
    export let scores: number[] = [];

    let cornerModifiers = ["+", "-", ""];
    let colors = ["red", "blue"];
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

    function setCornerModifier(modifier: string) {
        currentCornerModifier = modifier;
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

<div class="container">
    <div class="stack-wrapper">
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
    </div>

    <!-- Side Buttons -->
    <div class="side-controls">
        {#each colors as color, _}
            {#if color != excludedColor}
                <button class="{color}-btn" on:click={() => addSquare(color)}
                ></button>
            {/if}
        {/each}
        <button
            class="minus-btn"
            on:click={() => {
                removeSquare();
            }}
        >
        </button>
    </div>
</div>

{#if cornerModifier}
    <div class="bottom-controls">
        {#each cornerModifiers as modifier, _}
            <button
                class={modifier === "+"
                    ? "plus-btn"
                    : modifier === "-"
                      ? "minus-btn"
                      : "tilde-btn"}
                style="border: {currentCornerModifier == modifier
                    ? '5px solid black'
                    : ''}"
                on:click={() => setCornerModifier(modifier)}
            >
            </button>
        {/each}
    </div>
{/if}

<style>
    @import "./buttons.css";

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
    }

    .stack {
        display: flex;
        flex-direction: column;
        gap: 10px;
        align-items: center;
    }

    .ring {
        width: 200px;
        height: 20px;
        border: none;
        display: flex;
        justify-content: center;
        align-items: center;
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
