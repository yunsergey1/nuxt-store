<template>
    <main>
        <div class="container">
        <Navbar/>

            <Nuxt />
        </div>

        <Footer/>
    </main>
</template>


<style>
    body, #__nuxt, #__layout, main {
        height: 100%;
    }
    .container {
        min-height: calc(100vh - 56px);
    }
</style>