<script lang="ts">
    import Stack from "./Stack.svelte";

    let autoWinners = ["red", "tie", "blue"];

    let currentAutoWinner = "tie";

    function setAutoWinner(autoWinner: string) {
        currentAutoWinner = autoWinner;
    }

    let scores: number[] = [0, 0];

    let mobileGoalScores: number[][] = [];

    $: {
        let redScore = 0;
        let blueScore = 0;

        if (currentAutoWinner === "red") {
            redScore += 6;
        } else if (currentAutoWinner === "blue") {
            blueScore += 6;
        } else {
            redScore += 3;
            blueScore += 3;
        }

        for (let goalScore of mobileGoalScores) {
            if (goalScore === undefined || goalScore.length === 0) continue;
            redScore += goalScore[0];
            blueScore += goalScore[1];
        }

        if (redScore < 0) {
            redScore = 0;
        }

        if (blueScore < 0) {
            blueScore = 0;
        }

        scores = [redScore, blueScore];
    }
</script>

<table class="scoring-table">
    <thead>
        <tr></tr>
        <tr>
            <td colspan="2">
                Auton Bonus
                <div class="bonus-selector">
                    {#each autoWinners as autoWinner, _}
                        <button
                            class={autoWinner === "red"
                                ? "red-btn"
                                : autoWinner === "blue"
                                  ? "blue-btn"
                                  : "tilde-btn"}
                            style="border: {currentAutoWinner == autoWinner
                                ? '5px solid white'
                                : ''}"
                            on:click={() => setAutoWinner(autoWinner)}
                            aria-label="{autoWinner} bonus"
                        ></button>
                    {/each}
                </div>
            </td>
            <td colspan="1" class="score">
                <span style="color: red">{scores[0]}</span>
                :
                <span style="color: blue">{scores[1]}</span>
            </td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><Stack bind:scores={mobileGoalScores[0]} name="Goal 1" /></td>
            <td>
                <Stack
                    bind:scores={mobileGoalScores[1]}
                    cornerModifier={false}
                    name="Wall 1"
                />
            </td>
            <td><Stack bind:scores={mobileGoalScores[2]} name="Goal 3"/></td>
        </tr>
        <tr>
            <td><Stack bind:scores={mobileGoalScores[3]} name="Goal 2"/></td>
            <td>
                <Stack
                    bind:scores={mobileGoalScores[4]}
                    cornerModifier={false}
                    name="Wall 2"
                />
            </td>
            <td><Stack bind:scores={mobileGoalScores[5]} name="Goal 4"/></td>
        </tr>
        <tr>
            <td>
                <Stack
                    bind:scores={mobileGoalScores[6]}
                    size={2}
                    cornerModifier={false}
                    excludedColor="blue"
                    name="Red"
                />
            </td>
            <td><Stack bind:scores={mobileGoalScores[7]} name="Goal 5"/></td>
            <td>
                <Stack
                    bind:scores={mobileGoalScores[8]}
                    size={2}
                    cornerModifier={false}
                    excludedColor="red"
                    name="Blue"
                />
            </td>
        </tr>
    </tbody>
</table>

<svelte:head>
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" />
    <link
        href="https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap"
        rel="stylesheet"
    />
</svelte:head>

<style>
    @import "./buttons.css";

    :global(*) {
        margin: 0;
        padding: 0;
        overflow-x: hidden;
        border-collapse: collapse;
        font-family: "Montserrat";
    }

    :global(body) {
        background-color: rgb(180, 180, 180);
    }

    .scoring-table {
        font-size: 50px;
        width: 100%;
        height: 100vh;
        border-collapse: collapse;
        table-layout: fixed;
    }

    .scoring-table td {
        border: 5px solid black;
        vertical-align: top;
        text-align: center;
        padding: 10px;
        position: relative;
        width: 33.33vw;
        box-sizing: border-box;
    }

    .scoring-table thead td {
        width: 66.66vw;
    }

    .bonus-selector {
        display: flex;
        align-items: center;
        gap: 10px;
        justify-content: center;
    }

    .score {
        font-size: 80px;
    }
</style>
