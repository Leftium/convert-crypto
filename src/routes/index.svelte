<svelte:head>
    <link href='/fa/css/all.css' rel='stylesheet'>
</svelte:head>

<script context="module" type="text/coffeescript">
    _load = ({page, fetch, session, stuff}) =>
        response = await fetch 'https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=usd,krw'
        return response.json().then (data) -> return
            props:
                satsPerUsd: 100_000_000/data.bitcoin.usd
                satsPerKrw: 100_000_000/data.bitcoin.krw
    `export const load = _load`
</script>

<script type="text/coffeescript">
    # Props from load()
    `export let satsPerUsd`
    `export let satsPerKrw`

    import ClipboardInput from '$lib/components/ClipboardInput.svelte'

    `let inputQuery = null`
    `let parsedQuery = null`
    `let formattedParsedQuery = null`

    `let formattedParsedQueryComponent = null`

    `let satoshiFromBtcComponent = null`
    `let satoshiFromUsdComponent = null`
    `let satoshiFromKrwComponent = null`

    `let hodlBotLowPriceComponent = null`
    `let hodlBotHighPriceComponent = null`
    `let hodlBotTrailingDownComponent = null`

    `export let query = ''`

    parseQuery = (q) ->
        v = q.replace /[^0-9.]/g, ''
        v = parseFloat(v, 10) or 1  # Default to value of 1

    `$: parsedQuery = parseQuery(query)`
    `$: formattedParsedQuery = parsedQuery.toLocaleString()`

    convertWithRate = (v, rate) ->
        v = v * rate
        v.toLocaleString()

    getSatoshiFromBtc = (q) -> convertWithRate q, 100_000_000  # SATS/BTC
    getSatoshiFromUsd = (q) -> convertWithRate q, satsPerUsd
    getSatoshiFromKrw = (q) -> convertWithRate q, satsPerKrw

    getHodlBotLowPrice     = (q) -> q * (1 - 0.0037*22)
    getHodlBotHighPrice    = (q) -> q * (1 + 0.0037*7)
    getHodlBotTrailingDown = (q) -> getHodlBotLowPrice(q) * 0.9

    handlePaste = (e) ->
        data = (event.clipboardData || window.clipboardData).getData('text');
        if data
            inputQuery.value = query = data
            parsedQuery = parseQuery data
            formattedParsedQueryComponent.textContent = parsedQuery.toLocaleString()

            satoshiFromBtcComponent.value = getSatoshiFromBtc(parsedQuery)
            satoshiFromUsdComponent.value = getSatoshiFromUsd(parsedQuery)
            satoshiFromKrwComponent.value = getSatoshiFromKrw(parsedQuery)

            hodlBotLowPriceComponent.value     = getHodlBotLowPrice(parsedQuery)
            hodlBotHighPriceComponent.value    = getHodlBotHighPrice(parsedQuery)
            hodlBotTrailingDownComponent.value = getHodlBotTrailingDown(parsedQuery)

    nullHandler = (e) -> console.log '(nullHandler)'

</script>

<style>
    :global(.results button) {
        width: 130px;
    }

    input {
        font-family: inherit;
        font-size: inherit;
        padding: 0.4em;
        margin: 0 0 0.5em 0;
        box-sizing: border-box;
        border: 1px solid #ccc;
        border-radius: 2px;

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

<div bind:this={formattedParsedQueryComponent}>
    {formattedParsedQuery}
</div>

<div class="results">
    <fieldset>
        <legend>Satoshi</legend>
        <div>
            <ClipboardInput
                bind:this={satoshiFromBtcComponent}
                label='BTC&#8680;SAT'
                value={getSatoshiFromBtc(parsedQuery)} />
        </div>

        <div>
            <ClipboardInput
                bind:this={satoshiFromUsdComponent}
                label='USD&#8680;SAT'
                value={getSatoshiFromUsd(parsedQuery)} />
        </div>

        <div>
            <ClipboardInput
                bind:this={satoshiFromKrwComponent}
                label='KRW&#8680;SAT'
                value={getSatoshiFromKrw(parsedQuery)} />
        </div>
    </fieldset>

    <fieldset>
        <legend>HODL Bot</legend>
        <div>
            <ClipboardInput
                bind:this={hodlBotLowPriceComponent}
                label='Low Price'
                value={getHodlBotLowPrice(parsedQuery)} />
        </div>

        <div>
            <ClipboardInput
                bind:this={hodlBotHighPriceComponent}
                label='High Price'
                value={getHodlBotHighPrice(parsedQuery)} />
        </div>

        <div>
            <ClipboardInput
                bind:this={hodlBotTrailingDownComponent}
                label='Trailing Down'
                value={getHodlBotTrailingDown(parsedQuery)} />
        </div>
    </fieldset>

</div>