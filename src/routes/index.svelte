<script lang='ts'>
    import Select from '../components/select.svelte';
    import randomColor from 'randomcolor'
    import { shuffle } from 'lodash-es';
    import CardBox from '../components/cardBox.svelte';

    let selected: number = 16;
    let amounts: Array<number> = [4, 16, 36, 64];
    let cards = []
    let wrong = 0;
    let correct = 0;

    $:{
        selected;
        reset();
    }

    let reset = () => {
        wrong = 0;
        correct = 0;
        cards = getCards(selected);
    }

    let getCards = (selected: number) => {  
        const colorTypes = Array(selected / 2)
            .fill(0)
            .map(() => randomColor({luminosity: 'bright'}));
        
        const colors = colorTypes.concat(colorTypes);

        const cards = Array(selected)
            .fill(0)
            .map((_, i) => ({id: i, color: colors.pop(), isMatched: false, isTurned: false}));
        
        return shuffle(cards);
    }

    let turnCard = (card) => {

        let notMatchedTurnedCards = cards.filter((c) => (!c.isMatched && c.isTurned));
        if (notMatchedTurnedCards.filter((c) => card.id == c.id).length != 0) return;
        if (notMatchedTurnedCards.length < 2) {
            card.isTurned = true;
            cards = cards;
            notMatchedTurnedCards = cards.filter((c) => (!c.isMatched && c.isTurned));
        }
        if (notMatchedTurnedCards.length == 2 && (notMatchedTurnedCards.filter((c) => card.id == c.id).length != 0) ) {
            if(notMatchedTurnedCards[0].color === notMatchedTurnedCards[1].color) {
                notMatchedTurnedCards.forEach((c) => (c.isMatched = true));
                correct++;

                if(cards.filter((c) => !c.isMatched).length === 0) {
                    setTimeout(() => {
                        alert('You won!');
                        reset();},
                    500);
                }
            } 
            else {
                wrong++;
                setTimeout(() => notMatchedTurnedCards.forEach((c) => 
                    {c.isTurned = false; cards=cards}
                    ), 1000);
            }
        }               
    }
</script>

<svelte:head>
    <title>Memory Game</title>
</svelte:head>

<h1 class='text-center text-5xl font-bold text-gray-900 p-2'>Memory Game</h1>
<div class='relative font-semibold left-5 right-5 w-max text-2xl'>
    <Select bind:selected={selected} amounts={amounts}/>
    <div>Correct Guesses: <span class='text-green-500'>{correct}</span></div>
    <div>Wrong Guesses: <span class='text-red-500'>{wrong}</span></div>
    <button on:click={reset} class='relative font-semibold bg-gray-900 rounded-3xl text-white px-3 translate-y-2'>Reset</button>
</div>

<CardBox selected={selected} cards={cards} turnCard={turnCard} />