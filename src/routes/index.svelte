<svelte:head>
    <link href='/fa/css/all.css' rel='stylesheet'>
</svelte:head>

<script lang="coffee">
    import ClipboardInput from '$lib/components/ClipboardInput.svelte'

    console.log 'CoffeeScript!'

    `let query = ''`

    satoshiFromBtc = (q) ->
        v = parseFloat (q or 1), 10
        (v * 1_000_000_000).toLocaleString()


</script>


<input bind:value={query}>


<div>
    <ClipboardInput label='BTC&#8680;SAT' value='{satoshiFromBtc(query)}'></ClipboardInput>
</div>
