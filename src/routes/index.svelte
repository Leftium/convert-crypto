<svelte:head>
    <link href='/fa/css/all.css' rel='stylesheet'>
</svelte:head>


<script lang="coffee">
    import ClipboardInput from '$lib/components/ClipboardInput.svelte'

    `let inputQuery = null`
    `let satoshiFromBtcComponent = null`
    `let satoshiFromUsdComponent = null`
    `let satoshiFromKrwComponent = null`

    `export let query = ''`

    convertWithRate = (q, rate) ->
        q = q.replace /[^0-9.]/g, ''
        v = parseFloat (q or 1), 10
        v = v * rate
        v.toLocaleString()

    getSatoshiFromBtc = (q) -> convertWithRate q, 100_000_000        # SATS/BTC
    getSatoshiFromUsd = (q) -> convertWithRate q, 1_736              # SATS/USD
    getSatoshiFromKrw = (q) -> convertWithRate q, 1.4568791948924265 # SATS/KRW

    `$: satoshiFromBtc = getSatoshiFromBtc(query)`
    `$: satoshiFromUsd = getSatoshiFromUsd(query)`
    `$: satoshiFromKrw = getSatoshiFromKrw(query)`

    handlePaste = (e) ->
        data = (event.clipboardData || window.clipboardData).getData('text');
        if data
            inputQuery.value = data
            query = data
            satoshiFromBtcComponent.value = getSatoshiFromBtc(query)
            satoshiFromUsdComponent.value = getSatoshiFromUsd(query)
            satoshiFromKrwComponent.value = getSatoshiFromKrw(query)

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

<div>
    <ClipboardInput
        bind:this={satoshiFromUsdComponent}
        label='USD&#8680;SAT'
        value={satoshiFromUsd} />
</div>

<div>
    <ClipboardInput
        bind:this={satoshiFromKrwComponent}
        label='KRW&#8680;SAT'
        value={satoshiFromKrw} />
</div>
