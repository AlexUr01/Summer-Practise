<script context="module">
    /**
    * Функция, определяющая выбранный автомобиль в окне по параметрам в адресной строке
    */
    export async function preload({ params }) {
        const res = await this.fetch(`cars/${params.car}.json`);
        const data = await res.json();

        if (res.status === 200) {
            return { car: data };
        } else {
            this.error(res.status, data.message);
        }
    }
</script>

<svelte:head>
    <title>Cars Catalog | {car.brand} {car.model}</title>
</svelte:head>

<script>
    import Head from "../../components/Head.svelte";

    /**
    * Переменная, хранящая объект выбранного автомобиля
    * @type {object}
    */
    export let car;
</script>

<Head needFavoriteButton={false}/>
<div class="car-base">
    <img src={car.img} alt="" width="350">
    <div class="base-info">
        <div class="car-name">{car.brand} {car.model}</div>
        <div class="car-price">Цена:<div style="color: grey; margin-left: 10px;">{car.price}руб.</div></div>
    </div>
</div>
<hr>
<div class="characteristic">
    <div class="characteristic-name">Год выпуска:</div>
    <div class="characteristic-value">{car.release}</div>
</div>
<div class="characteristic">
    <div class="characteristic-name">Привод:</div>
    <div class="characteristic-value">{car.drive}</div>
</div>
<div class="characteristic">
    <div class="characteristic-name">Тип кузова:</div>
    <div class="characteristic-value">{car.body}</div>
</div>
<div class="characteristic">
    <div class="characteristic-name">Трансмиссия:</div>
    <div class="characteristic-value">{car.transmission}</div>
</div>
<div class="characteristic">
    <div class="characteristic-name">Объем двигателя:</div>
    <div class="characteristic-value">{car.engineVolume} куб.см</div>
</div>
<div class="characteristic">
    <div class="characteristic-name">Мощность двигателя:</div>
    <div class="characteristic-value">{car.enginePower} л.с.</div>
</div>
<div class="characteristic">
    <div class="characteristic-name">Максимальная скорость:</div>
    <div class="characteristic-value">{car.maxSpeed} км/ч</div>
</div>

<style>
    .base-info {
        padding: 10px 0 10px 25px;
        width: 200px;
    }
    .car-base {
        display: flex;
        padding: 80px 0 0 calc(50% - 250px);
    }
    .car-name {
        text-decoration: none; 
        font-weight: bold;
    }
    .car-price {
        margin-top: 50px;
        font-size: 0.8em;
        display: flex;
    }
    .characteristic {
        display: flex;
        height: 25px;
    }
    .characteristic-name {
        position: absolute;
        padding-left: calc(50% - 250px);
    }
    .characteristic-value {
        position: absolute;
        padding-left: 50%;
    }
</style>