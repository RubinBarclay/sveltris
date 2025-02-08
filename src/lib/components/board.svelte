<script lang="ts">
import { generateBoardGrid } from "$lib/utils/board";

generateBoardGrid();
let grid: number[][] = $state(generateBoardGrid());

</script>

<div class="board">
    {#each grid as row}
        {#each row as cell}
            <div class={cell ? 'active' : ''}></div>
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
</style>