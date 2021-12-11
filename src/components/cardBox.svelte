<script>
    import Card from "./card.svelte";

    export let cards;
    export let selected;
    export let turnCard;
</script>

<div class='flex place-items-center justify-center'>
    <div class="grid p-4 gap-2" style="grid-template-columns: repeat({Math.pow(selected, 1/2)}, minmax(0, 1fr));">	
        {#each cards as card}
            <Card {card} on:click={turnCard(card)} />
        {/each}
    </div>
</div>
