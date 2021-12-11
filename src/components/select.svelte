<script>
    export let selected;
    export let amounts;
</script>

<label for='amount' class='relative'>Amount of Cards:
    <select id='amount' bind:value={selected} class='bg-white text-blue-500'>
        {#each amounts as n}
            <option value={n}>{n}</option>
        {/each}
    </select>
</label>