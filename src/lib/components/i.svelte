<script lang="ts">

    let grid: string[][] = [
        ['O', 'O', 'O', 'O'],
        ['O', 'O', 'O', 'O'],
        ['O', 'O', 'O', 'O'],
        ['O', 'O', 'O', 'O'],
    ]

    // const coordinates: number[][][] = [
    //     [[0, 0], [1, 0], [2, 0], [3, 0]],
    //     [[1, 0], [1, 1], [1, 2], [1, 3]],
    //     [[0, 2], [1, 2], [2, 2], [3, 2]],
    //     [[3, 0], [3, 1], [3, 2], [3, 3]],
    // ]

    const coordinates: number[][][] = [
        [[0, 1], [1, 1], [2, 1], [3, 1]],
        [[1, 3], [1, 2], [1, 1], [1, 0]],
        [[3, 2], [2, 2], [1, 2], [0, 2]],
        [[3, 0], [3, 1], [3, 2], [3, 3]],
    ]
    
    let counter = $state(0);

    // setInterval(() => {

    //     if (counter > 0) {
    //         for (let [x, y] of coordinates[counter - 1]) {
    //             grid[x][y] = 'O'
    //         }
    //     }

    //     for (let [x, y] of coordinates[counter]) {
    //         grid[x][y] = 'X'
    //     }

    //     console.log(grid)

    //     if (counter < 3) {
    //         counter += 1;
    //     } else {
    //         counter = 0;
    //     }
    // }, 4000);
</script>

<div class="grid">
    {#each coordinates[counter] as [x, y] , i}
        <div class="grid-row-{x} grid-column-{y} grid-cell-{i}"></div>
    {/each}
</div>

<button onclick={() => {
        if (counter < 3) {
            counter += 1;
        } else {
            counter = 0;
        }
    }
}>Rotate</button>

<style lang="scss">
@mixin grid-row($start) {
    grid-row: calc($start + 1) / span 1;
}

@mixin grid-column($start) {
    grid-column: calc($start + 1) / span 1;
}

.grid {
    height: 4rem;
    width: 4rem;
    display: grid;
    grid-template-rows: repeat(4, 1rem);
    grid-template-columns: repeat(4, 1rem);
    border: 1px solid black;

    &-row-0 { @include grid-row(0) }
    &-row-1 { @include grid-row(1) }
    &-row-2 { @include grid-row(2) }
    &-row-3 { @include grid-row(3) }

    &-column-0 { @include grid-column(0) }
    &-column-1 { @include grid-column(1) }
    &-column-2 { @include grid-column(2) }
    &-column-3 { @include grid-column(3) }

    &-cell-0 { background-color: red }
    &-cell-1 { background-color: blue }
    &-cell-2 { background-color: green }
    &-cell-3 { background-color: purple }
}
</style>