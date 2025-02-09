<script lang="ts">
import { generateBoardGrid } from "$lib/utils/board";

let grid: number[][] = $state(generateBoardGrid());

let play = $state(false);

let tetrominoRowIndex = $state(0);

// Game function

const start = () => {
    updateCells()
    // play = true;
}

const stop = () => {
    play = false;
}

const updateCells = () => {
    grid = generateBoardGrid();
    
    const [cell1Row, cell1Col] = iTetrominoData2.cell1Position;
    grid[cell1Row][cell1Col] = 1;
    
    const [cell2Row, cell2Col] = iTetrominoData2.cell2Position;
    grid[cell2Row][cell2Col] = 1;
    
    const [cell3Row, cell3Col] = iTetrominoData2.cell3Position;
    grid[cell3Row][cell3Col] = 1;
    
    const [cell4Row, cell4Col] = iTetrominoData2.cell4Position;
    grid[cell4Row][cell4Col] = 1;
}

$effect(() => {
    const interval = play ? setInterval(() => {
        // iTetrominoData.rowIndex += 1;
        tetrominoRowIndex += 1;
    }, 1000) : undefined;

    if (!play) {
        clearInterval(interval);
    }

    return () => {
        clearInterval(interval);
    };
});

$effect(() => {
    const { rowIndex, columnIndex } = iTetrominoData;

    grid[rowIndex][columnIndex] = 1;
})

// I Tetromino

let iTetrominoRotationCounter = $state(0);

let iTetrominoGrid: number[][] = [
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
]

const iTetrominoRotationCoordinates: number[][][] = [
    [[0, 1], [1, 1], [2, 1], [3, 1]],
    [[1, 3], [1, 2], [1, 1], [1, 0]],
    [[3, 2], [2, 2], [1, 2], [0, 2]],
    [[2, 0], [2, 1], [2, 2], [2, 3]],
]
console.log(JSON.stringify(iTetrominoRotationCoordinates))

let iTetrominoData2 = $state({
    leftSpace: 4,
    rightSpace: 5,
    position: 0,
    cell1Position: [0, 4], // [1, 6] [3, 5]
    cell2Position: [1, 4], // [1, 5]
    cell3Position: [2, 4], // [1, 4]
    cell4Position: [3, 4], // [1, 3]

    width: 4,
    height: 4,
})

let iTetrominoData = $state({
    width: 1,
    height: 4,
    rotationCoordinates: iTetrominoRotationCoordinates[0],
    rowIndex: 0,
    columnIndex: 0,
    leftMargin: 0,
    rightMargin: 3,
})

const rotateITetromino = () => {

    let nextRotationCounter = 0;

    if (iTetrominoRotationCounter < 3) {
        nextRotationCounter = iTetrominoRotationCounter + 1;
    } else {
        nextRotationCounter = 0;
    }

    const [cell1Row, cell1Col] = iTetrominoData2.cell1Position;

    const cell1RowDifference = iTetrominoRotationCoordinates[nextRotationCounter][0][0] - iTetrominoRotationCoordinates[iTetrominoRotationCounter][0][0];
    const cell1ColDifference = iTetrominoRotationCoordinates[nextRotationCounter][0][1] - iTetrominoRotationCoordinates[iTetrominoRotationCounter][0][1];
    
    const nextCell1Row = cell1Row + cell1RowDifference
    const nextCell1Col = cell1Col + cell1ColDifference

    console.log([cell1Row, cell1Col])
    console.log([nextCell1Row, nextCell1Col])

    iTetrominoData2.cell1Position = [nextCell1Row, nextCell1Col]

    const [cell2Row, cell2Col] = iTetrominoData2.cell2Position;

    const cell2RowDifference = iTetrominoRotationCoordinates[nextRotationCounter][1][0] - iTetrominoRotationCoordinates[iTetrominoRotationCounter][1][0];
    const cell2ColDifference = iTetrominoRotationCoordinates[nextRotationCounter][1][1] - iTetrominoRotationCoordinates[iTetrominoRotationCounter][1][1];

    const nextCell2Row = cell2Row + cell2RowDifference
    const nextCell2Col = cell2Col + cell2ColDifference

    console.log([cell2Row, cell2Col])
    console.log([nextCell2Row, nextCell2Col])
    
    iTetrominoData2.cell2Position = [nextCell2Row, nextCell2Col]

    const [cell3Row, cell3Col] = iTetrominoData2.cell3Position;

    const cell3RowDifference = iTetrominoRotationCoordinates[nextRotationCounter][2][0] - iTetrominoRotationCoordinates[iTetrominoRotationCounter][2][0];
    const cell3ColDifference = iTetrominoRotationCoordinates[nextRotationCounter][2][1] - iTetrominoRotationCoordinates[iTetrominoRotationCounter][2][1];

    const nextCell3Row = cell3Row + cell3RowDifference
    const nextCell3Col = cell3Col + cell3ColDifference

    console.log([cell3Row, cell3Col])
    console.log([nextCell3Row, nextCell3Col])

    iTetrominoData2.cell3Position = [nextCell3Row, nextCell3Col]

    const [cell4Row, cell4Col] = iTetrominoData2.cell4Position;

    const cell4RowDifference = iTetrominoRotationCoordinates[nextRotationCounter][3][0] - iTetrominoRotationCoordinates[iTetrominoRotationCounter][3][0];
    const cell4ColDifference = iTetrominoRotationCoordinates[nextRotationCounter][3][1] - iTetrominoRotationCoordinates[iTetrominoRotationCounter][3][1];

    const nextCell4Row = cell4Row + cell4RowDifference
    const nextCell4Col = cell4Col + cell4ColDifference

    console.log([cell4Row, cell4Col])
    console.log([nextCell4Row, nextCell4Col])

    iTetrominoData2.cell4Position = [nextCell4Row, nextCell4Col]

    updateCells()

    if (iTetrominoRotationCounter < 3) {
        iTetrominoRotationCounter += 1;
    } else {
        iTetrominoRotationCounter = 0;
    }
}
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
    <button onclick={rotateITetromino}>Rotate</button>
</div>

<div class="test">
    {#each iTetrominoGrid as row}
        {#each row as cell}
            {#if cell}
                <div class="cell-active"></div>
            {/if}
        {/each}
    {/each}
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