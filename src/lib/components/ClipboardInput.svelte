<script type='text/coffeescript'>
    export value = null
    export label = null

    handleClickButton = (event) ->
        listener = (e) ->
            e.clipboardData.setData 'text/plain', value
            e.preventDefault()

        document.addEventListener 'copy', listener
        document.execCommand 'copy'
        document.removeEventListener 'copy', listener

    handleFocus = (event) ->
        @select()
</script>


<style>
button {
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;
}

input {
    width: 100%;
    margin-top: 4px;

    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
    border-top-right-radius: 2px;
    border-bottom-right-radius: 2px;
}

.InputAddOn {
  display: flex;
}

.InputAddOn-field,
.InputAddOn-item {
    border: 1px solid rgba(147,128,108,.25);
    padding: 20.5em, 2.75em;
}

.InputAddOn-field {
  flex: 1;
  /* field styles */

}

.InputAddOn-field:not(:first-child) {
    border-left: 0;
}

.InputAddOn-item {
    /* item styles */
    color: #666666;
    font: inherit;
    font-weight: normal;

    margin-top: 4px;
    margin-bottom: 0.5em;
}
</style>


<div class='InputAddOn'>
    <button class='InputAddOn-item'
            on:click={handleClickButton}>
        <i class='fas fa-clipboard decoration-fa'></i>
        {label}
    </button>
    <input class='InputAddOn-field'
           value={value}
           on:focus={handleFocus}>
</div>


