<!-- Application imports -->
<script setup>

import axios from 'axios'

</script>

<!-- Application logic -->
<script>
export default {

    data() {
        return {
            info: null,
            book: null
        }
    },

    methods: {

        handleChangeBook: function (e) {
            this.book = e.target.value
        },

        fetchBooks: function (e) {
            e.preventDefault()
            axios
                .get(`https://openlibrary.org/search.json?q=${this.book}&sort=new`)
                .then(response => (this.info = response.data.docs))
        }
    }
}
</script>

<template>

    <body class="fontSerrat">

        <main>

            <!-- Container -->
            <div class="h-screen w-screen overflow-auto">

                <!-- Overlay -->
                <div class="absolute top-0 left-0 right-0 bottom-0 bg-black/80 z-[2]" />

                <!-- Background image -->
                <div
                    class="absolute top-0 left-0 right-0 bottom-0 bg-library bg-no-repeat bg-cover bg-center bg-fixed z-[1]" />

                <!-- Search -->
                <div
                    class=" max-w-[600px] relative flex flex-col justify-between items-center w-[90vw] m-auto pt-4 text-white z-[11]">

                    <!-- Books form -->
                    <form v-on:submit="fetchBooks"
                        class="flex justify-between items-center w-full m-3 p-3 bg-transparent border-4 border-gray-300 text-white rounded-2xl">
                        <div>
                            <input class="bg-transparent border-none text-white focus:outline-none text-xl" type="text"
                                placeholder="Buscar libro" v-on:change="handleChangeBook">
                        </div>
                        <button v-on:click="fetchBooks"><font-awesome-icon icon="fa-solid fa-magnifying-glass"
                                class="text-xl" /></button>
                    </form>
                </div>

                <!-- Books table -->
                <div v-if="info != null" class="relative rounded-div my-4 z-[11]">
                    <table class="w-full border-collapse text-[1em] min-w-[400px]">
                        <thead>
                            <tr class="bg-[#2C74B3] text-[#ffffff] text-left font-bold border-b">
                                <th class="py-[12px] px-[15px]">Libro</th>
                                <th class="py-[12px] px-[15px]">Autor(a)</th>
                                <th class="py-[12px] px-[15px]">Año</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="book in info"
                                class="border-b-[1px] border-[#dddddd] border-solid overflow-hidden">
                                <td class="py-[12px] px-[15px]">{{ book.title }}</td>
                                <td class="py-[12px] px-[15px]">{{ book.author_name?.toString() }}</td>
                                <td class="py-[12px] px-[15px]">{{ book.publish_year?.toString() }}</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </main>
    </body>

</template>