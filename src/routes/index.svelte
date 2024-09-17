<script context="module">
        /**
        * Функция для загрузки списка автомобилей с сервера
        * @type {object}
        */
        export function preload() {
            return this.fetch(`index.json`).then(r => r.json()).then(cars => {
                return { cars };
            });
        }
    </script>
    
    <script>
        import Head from "../components/Head.svelte";
        import Filter from "../components/Filter.svelte";
        import CarsList from "../components/CarsList.svelte";
        import { writable } from "svelte/store";
    
        /**
        * Переменная, в которой хранится список автомобилей
        * @type {object}
        */
        export let cars;
    
        /**
        * Переменная, в которой хранится список избранных автомобилей
        * @type {object}
        */
        let favorite;
    
        /**
        * Функция, связывающая переменную favorite со storage объектом для того чтобы
        * список добавленных в избранное автомобилей не сбрасывался при перезагрузке страницы
        */
        if (typeof window !== 'undefined') {
            favorite = writable(localStorage.getItem("favorite") || []);
            favorite.subscribe(val => localStorage.setItem("favorite", val));
            if (typeof $favorite == 'string')
                favorite.set($favorite.split(','));
        }
    
        /**
        * Переменная, хранящая список отображаемых автомобилей с учетом фильтрации
        */
        let filteredCars = [...cars]
    
        /**
        * Функция, применяющая фильтры к списку автомобилей
        */
        function applyFilter(event) {
            filteredCars = [...cars];
            if (event.detail.brands.length !== 0) {
                filteredCars = filteredCars.filter(car => event.detail.brands.indexOf(car.brand) !== -1);
            }
            filteredCars = filteredCars.filter(car => (car.price >= event.detail.minPrice) && (car.price <= event.detail.maxPrice));
            filteredCars = filteredCars.filter(car => (car.release >= event.detail.minYear) && (car.release <= event.detail.maxYear));
            if (event.detail.drives.length !== 0) {
                filteredCars = filteredCars.filter(car => event.detail.drives.indexOf(car.drive) !== -1);
            }
            filteredCars = filteredCars.filter(car => (car.engineVolume >= event.detail.minEngineVolume) && (car.engineVolume <= event.detail.maxEngineVolume));
            filteredCars = filteredCars.filter(car => (car.enginePower >= event.detail.minEnginePower) && (car.enginePower <= event.detail.maxEnginePower));
        }
    
        /**
        * Функция, добавляющая, либо удаляющая автомобиль из списка избранных при нажатии соответствующей кнопки
        */
        function chooseFavorite(event) {
            if ($favorite.indexOf(event.detail) === -1){
                favorite.set([...$favorite, event.detail]);
            } else {
                favorite.set($favorite.filter((n) => {return n !== event.detail}));
            }
        }
    
        /**
        * Переменная, отвечающая за показ избранных автомобилей
        * @type {boolean}
        */
        let showFavorite = false;
    </script>
    
    <svelte:head>
        <title>Cars Catalog</title>
    </svelte:head>
    
    <Head on:showFavorite={(event) => {showFavorite = event.detail}}/>
    <Filter cars={cars} on:filterOptions={applyFilter}/>
    <div class="cars-list">
        <CarsList cars={showFavorite ? filteredCars.filter(car => $favorite.indexOf(car.id) !== -1) : filteredCars} favorite={$favorite} on:chooseFavorite={chooseFavorite}/>
    </div>
    
    <style>
        .cars-list {
            padding: 50px 40px 0 calc(50% - 200px);
        }
    </style>