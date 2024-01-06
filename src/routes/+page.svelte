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
                    if (!playing)
                        return ;
                    await delay(2);
                    numbersToSort = [...array];
                }
            }
            if (!swapped)
                break ;
        }
    }

    const quickSort = async (array: number[], low: number = 0, high: number = array.length - 1) => 
    {
        if (!playing)
            return ;
        if (low >= high)
            return ;
        const pivotIndex = await partition(array, low, high);
        await quickSort(array, low, pivotIndex - 1);
        await quickSort(array, pivotIndex + 1, high);
    };

    async function partition (array: number[], low: number, high: number): Promise<number>
    {
        const pivot = array[high];
        let i = low - 1;

        for (let j = low; j < high; j++)
        {
            if (array[j] >= pivot)
                continue ;
            i++;
            const temp = array[i];
            array[i] = array[j];
            array[j] = temp;

            await delay(2);
            numbersToSort = [...array];
        }

        const temp = array[i + 1];
        array[i + 1] = array[high];
        array[high] = temp;

        await delay(2);
        numbersToSort = [...array];

        return i + 1;
    };
    
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
        let randomNumbers = Array.from({length: 100}, () => Math.floor(Math.random() * 100));
        return randomNumbers;
    }

    async function toggleAlgorithm(i: number)
    {
        if (playing)
        {
            playing = false;
            await delay(100);
            numbersToSort = generateRandomNumbers();
        }
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
            <button on:click={async () => {playing = false; await delay(100); numbersToSort = generateRandomNumbers();}}>Reset</button>
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