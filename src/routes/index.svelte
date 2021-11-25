<script lang="coffee">
    console.log 'CoffeeScript!'

    `let query = ''`

    satoshiFromBtc = (q) ->
        v = parseFloat (q or 1), 10
        (v * 1_000_000_000).toLocaleString()


</script>


<input bind:value={query}>

<p>{satoshiFromBtc(query)} SAT &#8678; BTC</p>
