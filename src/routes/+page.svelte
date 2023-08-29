<script lang="ts">

    let playing: boolean;
    let algorithms: Algorithm[];
    let numbersToSort: number[];
    type Algorithm = {
        name: string,
        selected: boolean,
        function: (array: number[]) => Promise<void>
    };

    function delay(ms: number): Promise<void> {
        return new Promise(resolve => setTimeout(resolve, ms));
    }

    const bubbleSort = async (array: number[]) =>
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
                    if (playing == false)
                        return ;
                    await delay(2);
                    numbersToSort = [...array];
                }
            }
            if (!swapped)
                break ;
        }
    }

    const quickSort = async (array: number[]) => 
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
                    if (playing == false)
                        return ;
                    await delay(2);
                    numbersToSort = [...array];
                }
            }
            if (!swapped)
                break ;
        }
    }
    
    algorithms = [{
        name: "bubble_sort",
        selected: true,
        function: bubbleSort
    },
    {
        name: "quick_sort",
        selected: false,
        function: quickSort
    }];
    playing = false;
    numbersToSort = generateRandomNumbers();

    function generateRandomNumbers(): number[]
    {
        let randomNumbers: number[];

        randomNumbers = Array.from({length: 100}, () => Math.floor(Math.random() * 100));
        console.log(randomNumbers);
        return randomNumbers;
    }

    function toggleAlgorithm(i: number)
    {
        algorithms.map(algorithm => algorithm.selected = false);
        algorithms[i].selected = true;
    }

    function playSelected()
    {
        playing = true;
        for (let algorithm of algorithms)
            if (algorithm.selected)
                algorithm.function(numbersToSort);
    }

</script>

<main>
    <h1>algorithm visualisr</h1>
    <section>
        <div class="row">
            <button class:selected={algorithms[0].selected}
                    on:click={() => toggleAlgorithm(0)}>
                Bubble Sort
            </button>
            <button class:selected={algorithms[1].selected}
                    on:click={() => toggleAlgorithm(1)}>
            Quick Sort</button>
        </div>
        <div class="graph">
            {#each numbersToSort as number}
                <div class="stick" style="height: {number * 3}px;"></div>
            {/each}
        </div>
        {#if !playing}
            <button on:click={() => {playSelected();}}>Play</button>
        {:else}
            <button on:click={() => {playing = false; numbersToSort = generateRandomNumbers();}}>Reset</button>
        {/if}
    </section>
</main>

<style>
    .graph {
    display: flex;
    align-items: flex-end;
    }

    .stick {
        background-color: var(--primary);
        width: 10px
    }
</style>