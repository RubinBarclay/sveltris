<script lang="ts">
import { generateBoardGrid } from "$lib/utils/board";
import tetrominoRotationCoordinates from "$lib/const/tetrominoRotationCoordinates";
import { onMount } from "svelte";

let grid: number[][] = $state(generateBoardGrid());

let play = $state(false);

let currentTetromino: "i" | "j" | "l" | "o" | "s" | "t" | "z" = $state("j");

let tetrominoRotationCounter = $state(0);

let tetrominoData = $state({
    rowIndex: 0,
    columnIndex: 0,
    cell1Position: [0, 0],
    cell2Position: [0, 0],
    cell3Position: [0, 0],
    cell4Position: [0, 0],
})

// Game function

const start = () => {
    updateCells();
    play = true;
}

const stop = () => {
    play = false;
}

const updateCells = () => {
    grid = generateBoardGrid();
    
    const [cell1Row, cell1Col] = tetrominoData.cell1Position;
    grid[cell1Row][cell1Col] = 1;
    
    const [cell2Row, cell2Col] = tetrominoData.cell2Position;
    grid[cell2Row][cell2Col] = 1;
    
    const [cell3Row, cell3Col] = tetrominoData.cell3Position;
    grid[cell3Row][cell3Col] = 1;
    
    const [cell4Row, cell4Col] = tetrominoData.cell4Position;
    grid[cell4Row][cell4Col] = 1;
}

const rotateTetromino = () => {

    let nextRotationCounter = 0;

    if (tetrominoRotationCounter < 3) {
        nextRotationCounter = tetrominoRotationCounter + 1;
    } else {
        nextRotationCounter = 0;
    }

    const currentTetrominoRotationCoordinates = tetrominoRotationCoordinates[currentTetromino];

    const [cell1Row, cell1Col] = tetrominoData.cell1Position;

    const cell1RowDifference = currentTetrominoRotationCoordinates[nextRotationCounter][0][0] - currentTetrominoRotationCoordinates[tetrominoRotationCounter][0][0];
    const cell1ColDifference = currentTetrominoRotationCoordinates[nextRotationCounter][0][1] - currentTetrominoRotationCoordinates[tetrominoRotationCounter][0][1];
    
    const nextCell1Row = cell1Row + cell1RowDifference;
    const nextCell1Col = cell1Col + cell1ColDifference

    tetrominoData.cell1Position = [nextCell1Row, nextCell1Col]

    const [cell2Row, cell2Col] = tetrominoData.cell2Position;

    const cell2RowDifference = currentTetrominoRotationCoordinates[nextRotationCounter][1][0] - currentTetrominoRotationCoordinates[tetrominoRotationCounter][1][0];
    const cell2ColDifference = currentTetrominoRotationCoordinates[nextRotationCounter][1][1] - currentTetrominoRotationCoordinates[tetrominoRotationCounter][1][1];

    const nextCell2Row = cell2Row + cell2RowDifference;
    const nextCell2Col = cell2Col + cell2ColDifference

    tetrominoData.cell2Position = [nextCell2Row, nextCell2Col]

    const [cell3Row, cell3Col] = tetrominoData.cell3Position;

    const cell3RowDifference = currentTetrominoRotationCoordinates[nextRotationCounter][2][0] - currentTetrominoRotationCoordinates[tetrominoRotationCounter][2][0];
    const cell3ColDifference = currentTetrominoRotationCoordinates[nextRotationCounter][2][1] - currentTetrominoRotationCoordinates[tetrominoRotationCounter][2][1];

    const nextCell3Row = cell3Row + cell3RowDifference;
    const nextCell3Col = cell3Col + cell3ColDifference

    tetrominoData.cell3Position = [nextCell3Row, nextCell3Col]

    const [cell4Row, cell4Col] = tetrominoData.cell4Position;

    const cell4RowDifference = currentTetrominoRotationCoordinates[nextRotationCounter][3][0] - currentTetrominoRotationCoordinates[tetrominoRotationCounter][3][0];
    const cell4ColDifference = currentTetrominoRotationCoordinates[nextRotationCounter][3][1] - currentTetrominoRotationCoordinates[tetrominoRotationCounter][3][1];

    const nextCell4Row = cell4Row + cell4RowDifference;
    const nextCell4Col = cell4Col + cell4ColDifference

    tetrominoData.cell4Position = [nextCell4Row, nextCell4Col]

    if (tetrominoRotationCounter < 3) {
        tetrominoRotationCounter += 1;
    } else {
        tetrominoRotationCounter = 0;
    }

    updateCells()
}

const moveTetromino = (direction: number) => {
    const { cell1Position, cell2Position, cell3Position, cell4Position } = tetrominoData;

    tetrominoData.cell1Position = [cell1Position[0], cell1Position[1] + direction];
    tetrominoData.cell2Position = [cell2Position[0], cell2Position[1] + direction];
    tetrominoData.cell3Position = [cell3Position[0], cell3Position[1] + direction];
    tetrominoData.cell4Position = [cell4Position[0], cell4Position[1] + direction];
    
    updateCells()
}

$effect(() => {
    const interval = play ? setInterval(() => {
        const { cell1Position, cell2Position, cell3Position, cell4Position } = tetrominoData;

        tetrominoData.cell1Position = [cell1Position[0] + 1, cell1Position[1]];
        tetrominoData.cell2Position = [cell2Position[0] + 1, cell2Position[1]];
        tetrominoData.cell3Position = [cell3Position[0] + 1, cell3Position[1]];
        tetrominoData.cell4Position = [cell4Position[0] + 1, cell4Position[1]];
        
        updateCells()
    }, 1000) : undefined;


    if (!play) {
        clearInterval(interval);
    }

    return () => {
        clearInterval(interval);
    };
});

onMount(() => {
    tetrominoData.cell1Position = tetrominoRotationCoordinates[currentTetromino][0][0];
    tetrominoData.cell2Position = tetrominoRotationCoordinates[currentTetromino][0][1];
    tetrominoData.cell3Position = tetrominoRotationCoordinates[currentTetromino][0][2];
    tetrominoData.cell4Position = tetrominoRotationCoordinates[currentTetromino][0][3];
})

</script>

<div class="board">
    {#each grid as row}
        {#each row as cell}
            <div class={cell ? 'active' : ''}></div>
        {/each}
    {/each}
</div>

<div class="btns">
    <button onclick={start}>Start</button>
    <button onclick={stop}>Stop</button>
    <button onclick={rotateTetromino}>Rotate</button>
    <button onclick={() => moveTetromino(-1)}>Left</button>
    <button onclick={() => moveTetromino(1)}>Right</button>
</div>

<style lang="scss">
.board {
    display: inline-grid;
    grid-template-columns: repeat(10, 1.5rem);
    grid-template-rows: repeat(18, 1.5rem);

    border: 1px solid black;

    > .active {
        background-color: blue;
    }
}

.btns {
    display: flex;
}

@mixin grid-row($start) {
    grid-row: calc($start + 1) / span 1;
}

@mixin grid-column($start) {
    grid-column: calc($start + 1) / span 1;
}

.test {
    display: inline-grid;
    grid-template-rows: repeat(4, 1.5rem);
    grid-template-columns: repeat(4, 1.5rem);
    border: 1px solid black;

    &-row-0 { @include grid-row(0) }
    &-row-1 { @include grid-row(1) }
    &-row-2 { @include grid-row(2) }
    &-row-3 { @include grid-row(3) }

    &-column-0 { @include grid-column(0) }
    &-column-1 { @include grid-column(1) }
    &-column-2 { @include grid-column(2) }
    &-column-3 { @include grid-column(3) }

    margin: 1rem;

    .cell {
        background-color: gray;
    }

    .cell-active {
        background-color: red;
    }
}
</style>