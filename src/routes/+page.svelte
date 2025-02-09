<script lang="ts">
import Board from "$lib/components/board.svelte";
import J from "$lib/components/j.svelte";

</script>

<div class="game-container">
    <Board />
    <J />
</div>

<style lang="scss">
:global(html) {
    background-color: teal;
}

:global(body) {
    display: flex;
    justify-content: center;
}

.game-container {
    display: flex;
    flex-direction: column;
    align-items: center;

    width: 400px;
    margin-top: 40px;
    padding: 1.25rem;
    background-color: white;
}
</style>