<script lang="ts">

    type State = "start" | "playing" | "done";
    let currentState: State = "start";

    let numbersToSort = Array.from({length: 100}, () => Math.floor(Math.random() * 100));

    function delay(ms: number): Promise<void> {
        return new Promise(resolve => setTimeout(resolve, ms));
    }

    async function bubbleSort(array: number[])
    {
        let swapped: boolean = false;
        for (let i = 0; i < array.length; i ++)
        {
            swapped = false;
            for (let j = 0; j < array.length - 1; j ++)
            {
                if (array[j] > array[j + 1])
                {
                    const temp = array[j];
                    array[j] = array[j + 1];
                    array[j + 1] = temp;
                    swapped = true;
                    await delay(5);
                    numbersToSort = [...array];
                }
            }
            if (!swapped)
            {
                currentState = "done";
                break ;
            }
        }
    }
</script>

<main>
    <h1>algorithm visualisr</h1>
    <section>
    {#if currentState === "start"}
        <button on:click={() => {currentState = "playing"; bubbleSort(numbersToSort);}}>Play</button>
    {/if}
    {#if currentState === "playing" || currentState === "done"}
        <div class="graph">
            {#each numbersToSort as number}
                <div class="stick" style="height: {number * 3}px;"></div>
            {/each}
        </div>
    {/if}
    </section>
</main>