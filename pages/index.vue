<template>
    <section>
        <h1 class="text-uppercase bg-warning bg-gradient mb-4">Welcome to NUXT STORE!</h1>

        <!-- ФОРМА ПОИСКА.  
        При нажатии на Enter и при клике на Search!  
        Запускаем функцию searchProduct() -->
        <form class="w-100 mb-3 mb-lg-0 me-lg-3 d-flex text-end" role="search">
            <input type="search" v-model="search" class="form-control" placeholder="Search..." aria-label="Search" @keypress.enter="searchProduct()">
            <button type="submit" @click.prevent="searchProduct()" class="btn btn-primary">Search!</button>
        </form>

        <!-- ПАГИНАЦИЯ.
        Номера страниц выведены через цикл 
        На каждой странице по 12 карточек, чтобы делилось на 2 и на 3 
        При нажатии на Prev запускаем функцию previousPage()
        При нажатии на Next запускаем функцию nextPage() 
        При нажатии на номер страницы запускаем gotoPage()-->
        <nav>
            <ul class="pagination justify-content-end">
                <li class="page-item">
                    <a class="page-link" href="#" @click.prevent="previousPage()">
                        Prev
                    </a>
                </li>
                <li class="page-item col-0" v-for="(item, idx) in pages" :key="idx">
                    <a class="page-link number" href="#" @click.prevent="gotoPage(idx)">
                        {{ idx + 1 }}
                    </a>
                </li>
                <li class="page-item">
                    <a class="page-link" href="#" @click.prevent="nextPage()">
                        Next
                    </a>
                </li>
            </ul>
        </nav>

        <!-- КОНТЕЙНЕР ДЛЯ КАРТОЧЕК С ТОВАРАМИ -->
        <div class="album pb-3 bg-light rounded">
            <div class="container position-relative pt-5">

                <!-- КАТЕГОРИИ ТОВАРОВ.
                Категории товаров выведены через цикл. Взяты из API
                При клике на All Categories запускается gotoAllCategories()
                При клике на название категории запускается gotoCategory()-->
                <details id="categories" class="mb-3 position-absolute top-0 end-0 rounded">
                    <summary class="bg-success bg-gradient text-white text-center rounded">
                        Categories
                    </summary>
                    <ul class="list-unstyled ">
                        <button @click.prevent="gotoAllCategories()">
                            <strong>All categories</strong>
                        </button>
                        <button v-for="(item) in categories" :key="item" @click.prevent="gotoCategory(item)">
                            {{ item }}
                        </button>
                    </ul>
                </details>

                <!-- КАРТОЧКИ ТОВАРОВ.
                Вывод карточек товаров сделан через цикл.
                На одной странице 12 карточек товаров. Это сокращает время загрузки изображений.
                При клике на Read more запускаем функцию openProduct() -->
                <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-3">
                    <div class="col" v-for="(product, idx) of products" :key="idx">
                        <div class="card">
                            <img v-bind:src='product.images[0]' class="card-img-top pt-2" v-bind:alt="product.title">
                            <div class="card-body">
                                <h5 class="card-title">{{ product.title }}</h5>
                                <p class="card-text">{{ product.description }}</p>
                                <h4>PRICE: {{ product.price }} $</h4>
                                <button class="btn btn-primary" @click.prevent="openProduct(idx)">Read more</button>
                            </div>
                        </div>
                    </div>
                </div>

            </div>
        </div>

        <!-- ПАГИНАЦИЯ.
        Номера страниц выведены через цикл 
        На каждой странице по 12 карточек, чтобы делилось на 2 и на 3 
        При нажатии на Prev запускаем функцию previousPage()
        При нажатии на Next запускаем функцию nextPage() 
        При нажатии на номер страницы запускаем gotoPage()-->
        <nav>
            <ul class="pagination justify-content-end">
                <li class="page-item">
                    <a class="page-link" href="#" @click.prevent="previousPage()">
                        Prev
                    </a>
                </li>
                <li class="page-item col-0" v-for="(item, idx) in pages" :key="idx">
                    <a class="page-link number" href="#" @click.prevent="gotoPage(idx)">{{ idx + 1 }}</a>
                </li>
                <li class="page-item">
                    <a class="page-link" href="#" @click.prevent="nextPage()">
                        Next
                    </a>
                </li>
            </ul>
        </nav>

    </section>
</template>

<script>

    export default {
        // Создал стейт для работы с изменяющимися данными.
        data() {
            return {
                page: 0, // Номер отображаемой страницы для пагинации
                pages: 9, // Количество страниц для пагинации
                amount: 12, // Количество карточек на странице
                products: [], // Массив с карточками
                url: `https://dummyjson.com/products?limit=12&skip=0`,
                total: 100, // Общее количество карточек
                categories: [], // массив категорий
                search: '', // строка для поиска
            }
        },

        async fetch() {
            // Получаем массив с карточками товаров через fetch
            this.products = await fetch(this.url)
                .then(res => res.json()) // парсим объект
                .then(res => res.products) // указываем на нужный массив в объекте
            // Получаем количество карточек на сервере
            this.total = await fetch(this.url)
                .then(res => res.json())
                .then(res => res.total)
                // считаем количество страниц для пагинации
                .then(this.pages = Math.ceil(this.total / this.amount))
            // Получаем массив с категориями товаров.
            this.categories = await fetch('https://dummyjson.com/products/categories')
                .then(res => res.json())
                // .then(res => console.log(res))
        },

        methods: {
            nextPage() {
                console.log(this.total)
                if (this.page <= (this.total - this.amount)) {
                    this.page += this.amount
                    this.url = `https://dummyjson.com/products?limit=12&skip=${this.page}`
                    this.$fetch()
                }
            },
            previousPage() {
                if (this.page > 0) {
                    this.page -= this.amount
                    this.url = `https://dummyjson.com/products?limit=12&skip=${this.page}`
                    this.$fetch()
                }
            },
            gotoPage(idx) {
                this.page = this.amount * idx
                this.url = `https://dummyjson.com/products?limit=12&skip=${this.page}`
                this.$fetch()
            },
            gotoCategory(item) {
                this.url = `https://dummyjson.com/products/category/${item}`
                document.getElementById('categories').removeAttribute('open')
                this.$fetch()
            },
            gotoAllCategories() {
                this.url = `https://dummyjson.com/products?limit=12&skip=0`
                document.getElementById('categories').removeAttribute('open')
                this.$fetch()
            },
            searchProduct() {
                this.url = `https://dummyjson.com/products/search?q=${this.search}`
                console.log(this.url)
                this.$fetch()
            },
            openProduct(idx) {
                idx += 1
                this.$router.push('/products/' + idx)
            },
        },
    }

</script>

<style scoped>
    img {
        height: 160px;
        object-fit: contain;
    }
    h4 {
        text-align: right;
        color: #dc3545;
    }
    @media (max-width: 576px) {
        .number {
            display: none;
        }
    }
    details {
        border-radius: 4px;
        padding: .5em .5em 0;
        width: 220px;
        z-index: 10;
        background: #fff;

    }
    details ul {
        margin-bottom: 0;
    }

    details li {
        height: 2rem;
        padding-left: .5rem;
        color: #fff;
    }
    details button {
        width: 100%;
        border: 0;
        background: transparent;
    }
    details button:hover {
        cursor: pointer;
        background: #e7f3ee;
    }

    summary {
        font-weight: bold;
        margin: -.5em -.5em 0;
        padding: .5em 0;
    }

    details[open] {
        padding: .5em;
        box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
    }

    details[open] summary {
        border-bottom: 1px solid #aaa;
        margin-bottom: .5em;
        padding: .5em;
        
    }

</style>