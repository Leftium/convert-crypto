<svelte:head>
    <link href='/fa/css/all.css' rel='stylesheet'>
</svelte:head>

<script context="module" lang="coffee">
    export load = ({page, fetch, session, stuff}) ->
        response = await fetch 'https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=usd,krw'
        return response.json().then (data) -> return
            props:
                satsPerUsd: 100_000_000/data.bitcoin.usd
                satsPerKrw: 100_000_000/data.bitcoin.krw
</script>

<script lang="coffee">
    # Props from load()
    `export let satsPerUsd`
    `export let satsPerKrw`

    import ClipboardInput from '$lib/components/ClipboardInput.svelte'

    `let inputQuery = null`
    `let satoshiFromBtcComponent = null`
    `let satoshiFromUsdComponent = null`
    `let satoshiFromKrwComponent = null`

    `export let query = ''`

    convertWithRate = (q, rate) ->
        q = q.replace /[^0-9.]/g, ''
        v = parseFloat(q, 10) or 1  # Default to value of 1
        v = v * rate
        v.toLocaleString()

    getSatoshiFromBtc = (q) -> convertWithRate q, 100_000_000  # SATS/BTC
    getSatoshiFromUsd = (q) -> convertWithRate q, satsPerUsd
    getSatoshiFromKrw = (q) -> convertWithRate q, satsPerKrw

    handlePaste = (e) ->
        data = (event.clipboardData || window.clipboardData).getData('text');
        if data
            inputQuery.value = query = data
            satoshiFromBtcComponent.value = getSatoshiFromBtc(query)
            satoshiFromUsdComponent.value = getSatoshiFromUsd(query)
            satoshiFromKrwComponent.value = getSatoshiFromKrw(query)

    nullHandler = (e) -> console.log '(nullHandler)'

</script>

<style>
    :global(.results button) {
        width: 110px;
    }

    input, button, select, textarea {
        font-family: inherit;
        font-size: inherit;
        padding: 0.4em;
        margin: 0 0 0.5em 0;
        box-sizing: border-box;
        border: 1px solid #ccc;
        border-radius: 2px;
    }

    input {
        width: 100%;
        margin-top: 4px;

        border-top-left-radius: 0;
        border-bottom-left-radius: 0;
        border-top-right-radius: 2px;
        border-bottom-right-radius: 2px;
    }
</style>

<svelte:body on:paste={handlePaste} />

<div>
    <input bind:this={inputQuery}
           bind:value={query}
           on:paste|preventDefault={nullHandler} />
</div>

<div class="results">
    <div>
        <ClipboardInput
            bind:this={satoshiFromBtcComponent}
            label='BTC&#8680;SAT'
            value={getSatoshiFromBtc(query)} />
    </div>

    <div>
        <ClipboardInput
            bind:this={satoshiFromUsdComponent}
            label='USD&#8680;SAT'
            value={getSatoshiFromUsd(query)} />
    </div>

    <div>
        <ClipboardInput
            bind:this={satoshiFromKrwComponent}
            label='KRW&#8680;SAT'
            value={getSatoshiFromKrw(query)} />
    </div>
</div>