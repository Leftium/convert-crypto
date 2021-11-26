<svelte:head>
    <link href='/fa/css/all.css' rel='stylesheet'>
</svelte:head>



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
    <input

           on:paste|preventDefault={nullHandler} />
</div>

<div class="results">
    <div>
        <ClipboardInput

            label='BTC&#8680;SAT'
 />
    </div>

    <div>
        <ClipboardInput

            label='USD&#8680;SAT'
 />
    </div>

    <div>
        <ClipboardInput

            label='KRW&#8680;SAT'
 />
    </div>
</div>