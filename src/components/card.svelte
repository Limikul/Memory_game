<script>
    export let card;

</script>

<div class='flip-inner relative rounded-xl'> 
    <div class='front absolute rounded-xl' class:turned={card.isTurned} on:click>
        <img src = "favicon.png" alt="Card" class='bg-gray-900 rounded-xl' />
    </div>
    <div class='back rounded-xl' style="--backside-color: {card.color}">
        {#if card.isMatched}
            <img src="checkmark.png" alt="Checkmark"/>
        {/if}
    </div> 
</div>

<style>

    .flip-inner{
        user-select: none;
        perspective: 500px;
        box-shadow: 2px 2px rgba(0,0,0,0.2);
        transition: all 0.1s ease;
    }

    img {
        pointer-events: none;
    }

    .front, .back {
        backface-visibility: hidden;
        width: calc(3vh + 2.9vw);
        height: calc(3vh + 2.9vw);
        transition: transform 0.3s ease-out, background-color 0.3s 0.2s linear;
    }

    .flip-inner:hover {
        box-shadow: 4px 4px rgba(0,0,0,0.2);
    }

    .front.turned {
        transition-duration: 1000ms;
        transform: rotateY(180deg);
    }

    .back {
        background-color: var(--backside-color);
        border: black;
        border-style: solid;
        border-width: medium;
    }

</style>

