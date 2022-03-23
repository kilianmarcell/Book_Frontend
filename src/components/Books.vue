<template>
    <p class="fs-1 pt-5 ps-5">Petrik Könyvtár Nyilvántartó</p>
    <div class="row p-5">
        <div v-for="b in books" :key="b.id" class="border border-white col-xl-4 col-md-6 col-12">
            <div>
                <p class="fs-2">{{ b.title }}</p>
                <p class="fs-2">{{ b.author }}</p>
                <p class="fs-4">Kiadási év: {{ b.publish_year }}</p>
                <p class="fs-4">Hossz: {{ b.page_count }} oldal</p>
                 <img :src="getImgUrl(b.author)">
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios"

export default {
    name: 'Books',

    data() {
        return {
            books: [],
            book: {
                title: "",
                author: "",
                publish_year: null,
                page_count: null,
            },
            add_new: false,
            saving: false
        }
    },
    
    methods: {
        async loadData() {
            await axios
                .get('http://127.0.0.1:8000/api/books')
                .then(response => (this.books = response.data))
                .catch(error => console.log(error))
        },

        getImgUrl(pet) {
            var images = require.context('../assets/szerzok/', false, /\.jpg$/)
            return images('./' + pet + ".jpg")
        }
    },
    
    mounted() {
        this.loadData()
    }
}
</script>

<style>

</style>