<svelte:head>
    <link href='/fa/css/all.css' rel='stylesheet'>
</svelte:head>


<script lang="coffee">
    import ClipboardInput from '$lib/components/ClipboardInput.svelte'

    `let inputQuery = null`
    `let satoshiFromBtcComponent = null`

    `export let query = ''`
    `export let satoshiFromBtcValue=0`

    getSatoshiFromBtc = (q) ->
        q = q.replace /,/g, ''
        v = parseFloat (q or 1), 10
        v = v * 100_000_000
        v.toLocaleString()

    `$: satoshiFromBtc = getSatoshiFromBtc(query)`

    handlePaste = (e) ->
        data = (event.clipboardData || window.clipboardData).getData('text');
        if data
            inputQuery.value = data
            query = data
            satoshiFromBtcComponent.value = getSatoshiFromBtc(query)

    nullHandler = (e) ->
        console.log 'nullHandler'
        null

</script>

<svelte:body on:paste={handlePaste} />

<input bind:this={inputQuery}
       bind:value={query}
       on:paste|preventDefault={nullHandler}>


<div>
    <ClipboardInput
        bind:this={satoshiFromBtcComponent}
        label='BTC&#8680;SAT'
        value={satoshiFromBtc} />
</div>
