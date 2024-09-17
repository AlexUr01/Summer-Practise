<script>
    import { createEventDispatcher } from 'svelte';

    export let needFavoriteButton = true;
    
    /**
    * Переменная, хранящая состояние кнопки показа избранных автомобилей
    * @type {boolean}
    */
    let favorite = false;

    /**
    * Проброс переменной, отвечающий за показ избранных автомобилей в родительский элемент
    */
    const dispatch = createEventDispatcher();
    function showFavorite() {
        if (favorite) {
            favorite = false;
        } else {
            favorite = true;
        }
        dispatch('showFavorite', favorite);
    }
</script>

<div class="head">
    <div></div>
    <div style="font-weight: bold;">Выбор Авто</div>
    <!-- svelte-ignore a11y-click-events-have-key-events a11y-no-static-element-interactions -->
    <div class="buttons">
        {#if (needFavoriteButton)}
            <div class="button" on:click={showFavorite} style="color:{(favorite) ? "#c9cf23" : "inherit"};">{(favorite) ? "★" : "☆"}</div>
        {/if}
    </div>
</div>

<style type="text/css">
    .head {
        align-items: center;
        box-shadow: 0 0 5px;
        border-radius: 0 0 20px 20px; 
        padding: 0 20px;
        background: linear-gradient(to top, #795092, #D590FF);
        position: fixed; 
        display: flex;
        width: calc(100% - 60px);
        height: 40px;
        margin: 0 10px; 
        justify-content: space-between;
    }
    .button {
        cursor: pointer; 
        align-items: center;
        height: 40px; 
        padding: 0 10px 0 10px; 
        font-size: 1.5em; 
        display: flex;
    }
    .button:hover {
        box-shadow: 0 0 10px inset #56386899;
    }
    .buttons {
        display: flex;
    }
</style>