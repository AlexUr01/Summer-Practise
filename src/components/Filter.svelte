<script>
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    /**
    * Функция, пробрасывающая параметры, примененные в фильтре в родительский элемент
    */
    function applyFilter() {
        dispatch('filterOptions', {
            brands: Object.keys(uniqueBrands).filter(brand => uniqueBrands[brand] === true),
            minPrice: currentMinPrice,
            maxPrice: currentMaxPrice,
            minYear: currentMinYear,
            maxYear: currentMaxYear,
            drives: Object.keys(uniqueDrives).filter(drive => uniqueDrives[drive] === true),
            minEngineVolume: currentMinEngineVolume,
            maxEngineVolume: currentMaxEngineVolume,
            minEnginePower: currentMinEnginePower,
            maxEnginePower: currentMaxEnginePower
        });
    }

    /**
    * Переменная, хранящая список автомобилей
    * @type {object}
    */
    export let cars;
    cars = [...cars];

    /**
    * Переменные, , отвечающие за параметры фильтров
    */
    let uniqueBrands = {}
    Array.from(new Set(cars.map(item => item.brand))).forEach(element => uniqueBrands[element] = false)

    const minPrice = cars.sort((a, b) => a.price > b.price ? 1 : -1)[0].price;
    const maxPrice = cars[cars.length - 1].price;
    let currentMinPrice = minPrice;
    let currentMaxPrice = maxPrice;

    const minYear = cars.sort((a, b) => a.release > b.release ? 1 : -1)[0].release;
    const maxYear = cars[cars.length - 1].release;
    let currentMinYear = minYear;
    let currentMaxYear = maxYear;

    let uniqueDrives = {}
    Array.from(new Set(cars.map(item => item.drive))).forEach(element => uniqueDrives[element] = false)

    const minEngineVolume = cars.sort((a, b) => a.engineVolume > b.engineVolume ? 1 : -1)[0].engineVolume;
    const maxEngineVolume = cars[cars.length - 1].engineVolume;
    let currentMinEngineVolume = minEngineVolume;
    let currentMaxEngineVolume = maxEngineVolume;

    const minEnginePower = cars.sort((a, b) => a.enginePower > b.enginePower ? 1 : -1)[0].enginePower;
    const maxEnginePower = cars[cars.length - 1].enginePower;
    let currentMinEnginePower = minEnginePower;
    let currentMaxEnginePower = maxEnginePower;
</script>

<div class="filter">
    <div class="filter-element">
        <div class="filter-element-title">Марка</div>
        <div class="filter-element-options">
            {#each Object.keys(uniqueBrands) as brand}
                <div style="display: flex;">
                    <div style="width: 100px;">{brand}</div>
                    <div><input type=checkbox bind:checked={uniqueBrands[brand]}></div>
                </div>
            {/each}
        </div>
    </div>
    <div class="filter-element">
        <div class="filter-element-title">Цена</div>
        <div class="filter-element-options">
            <div>
                <label>
                    <div>
                        от: <input type=number bind:value={currentMinPrice} min={minPrice} max={currentMaxPrice}>руб.
                    </div>
                    <input type=range bind:value={currentMinPrice} min={minPrice} max={currentMaxPrice}>
                </label>
            </div>
            <div>
                <label>
                    <div>
                        до: <input type=number bind:value={currentMaxPrice} min={currentMinPrice} max={maxPrice}>руб.
                    </div>
                    <input type=range bind:value={currentMaxPrice} min={currentMinPrice} max={maxPrice}>
                </label>
            </div>
        </div>
    </div>
    <div class="filter-element">
        <div class="filter-element-title">Год выпуска</div>
        <div class="filter-element-options">
            <div>
                <label>
                    <div>
                        от: <input type=number bind:value={currentMinYear} min={minYear} max={currentMaxYear}>
                    </div>
                    <input type=range bind:value={currentMinYear} min={minYear} max={currentMaxYear}>
                </label>
            </div>
            <div>
                <label>
                    <div>
                        до: <input type=number bind:value={currentMaxYear} min={currentMinYear} max={maxYear}>
                    </div>
                    <input type=range bind:value={currentMaxYear} min={currentMinYear} max={maxYear}>
                </label>
            </div>
        </div>
    </div>
    <div class="filter-element">
        <div class="filter-element-title">Привод</div>
        <div class="filter-element-options">
            {#each Object.keys(uniqueDrives) as drive}
                <div style="display: flex;">
                    <div style="width: 100px;">{drive}</div>
                    <div><input type=checkbox bind:checked={uniqueDrives[drive]}></div>
                </div>
            {/each}
        </div>
    </div>
    <div class="filter-element">
        <div class="filter-element-title">Объем двигателя</div>
        <div class="filter-element-options">
            <div>
                <label>
                    <div>
                        от: <input type=number bind:value={currentMinEngineVolume} min={minEngineVolume} max={currentMaxEngineVolume}>куб.см
                    </div>
                    <input type=range bind:value={currentMinEngineVolume} min={minEngineVolume} max={currentMaxEngineVolume}>
                </label>
            </div>
            <div>
                <label>
                    <div>
                        до: <input type=number bind:value={currentMaxEngineVolume} min={currentMinEngineVolume} max={maxEngineVolume}>куб.см
                    </div>
                    <input type=range bind:value={currentMaxEngineVolume} min={currentMinEngineVolume} max={maxEngineVolume}>
                </label>
            </div>
        </div>
    </div>
    <div class="filter-element">
        <div class="filter-element-title">Мощность двигателя</div>
        <div class="filter-element-options">
            <div>
                <label>
                    <div>
                        от: <input type=number bind:value={currentMinEnginePower} min={minEnginePower} max={currentMaxEnginePower}>л.с.
                    </div>
                    <input type=range bind:value={currentMinEnginePower} min={minEnginePower} max={currentMaxEnginePower}>
                </label>
            </div>
            <div>
                <label>
                    <div>
                        до: <input type=number bind:value={currentMaxEnginePower} min={currentMinEnginePower} max={maxEnginePower}>л.с.
                    </div>
                    <input type=range bind:value={currentMaxEnginePower} min={currentMinEnginePower} max={maxEnginePower}>
                </label>
            </div>
        </div>
    </div>
    <button on:click={applyFilter}>Применить</button>
</div>

<style type="text/css">
    button {
        cursor: pointer;
    }

    input {
        font-size: 0.8em;
    }

    .filter {
        padding: 15px 20px 15px calc(50% - 450px);
        margin-top: 50px;
        border-right: 1px solid  #595959;
        position: fixed;
        width: 150px;
    }

    .filter-element {
        margin-bottom: 12px;
    }

    .filter-element-title {
        font-size: 0.8em;
        font-weight: bold;
    }

    .filter-element-options {
        font-size: 0.7em;
    }
</style>