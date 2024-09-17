<script>
    /**
    * Переменная, хранящая весь список автомобилей
    * @type {object}
    */
    export let cars;
    
    /**
    * Список id избранных автомобилей
    * @type {object}
    */
    export let favorite = [];

    const dispatch = createEventDispatcher();

    /**
    * Проброс id выбранного избранным автомобиля в родительский элемент
    */
    import { createEventDispatcher } from 'svelte';
    function chooseFavorite(carID) {
        dispatch('chooseFavorite', carID);
    }
</script>

<div>
    {#each cars as car}
        <div class="car">
            <img src={car.img} alt="" width="250">
            <div class="car-info">
                <a class="car-name" href="car/{car.id}">{car.brand} {car.model}</a>
                <div class="car-price">Цена:<div style="color: grey; margin-left: 10px;">{car.price}руб.</div></div>
            </div>
            <!-- svelte-ignore a11y-click-events-have-key-events a11y-no-static-element-interactions -->
            <div on:click={() => chooseFavorite(car.id)} class="favorite" style="color:{(favorite.indexOf(car.id) !== -1) ? "#c9cf23" : "inherit"};">
                {(favorite.indexOf(car.id) !== -1) ? "★" : "☆"}
            </div>
        </div>
    {/each}
</div>

<style>
    .favorite {
        font-size: 1.2em;
        cursor: pointer;
    }
    .car {
        display: flex;
        padding: 10px;
    }
    .car-name {
        text-decoration: none;
        font-weight: bold;
    }
    .car-name:hover {
        text-decoration: underline;
    }
    .car-price {
        margin-top: 50px;
        font-size: 0.8em;
        display: flex;
    }
    .car-info {
        padding: 10px 0 10px 25px;
        width: 200px;
    }
</style>