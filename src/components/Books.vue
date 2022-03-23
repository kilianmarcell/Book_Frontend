<template>
    <p class="fs-1 pt-5 ps-5">Petrik Könyvtár Nyilvántartó</p>
    <div class="row p-5">
        <div v-for="b in books" :key="b.id" class="border border-dark col-xl-4 col-md-6 col-12 p-5">
            <div>
                <p class="fs-2">{{ b.title }}</p>
                <p class="fs-2">{{ b.author }}</p>
                <p class="fs-4">Kiadási év: {{ b.publish_year }}</p>
                <p class="fs-4">Hossz: {{ b.page_count }} oldal</p>
                 <img class="text-center d-block mx-auto" :src="getImgUrl(b.author)">
            </div>
        </div>
        <div class="border border-dark w-100 mt-5 p-5 d-flex flex-column m-auto fs-3">
            <div class="row w-50 m-auto p-2">
                <p class="w-50">Cím:</p>
                <input v-model="this.book.title" class="w-50" type="text">
            </div>
            <div class="row w-50 m-auto p-2">
                <p class="w-50">Szerző:</p>
                <input v-model="this.book.author" class="w-50" type="text">
            </div>
            <div class="row w-50 m-auto p-2">
                <p class="w-50">Kiadási év:</p>
                <input v-model="this.book.publish_year" class="w-50" type="text">
            </div>
            <div class="row w-50 m-auto p-2">
                <p class="w-50">Hossz:</p>
                <input v-model="this.book.page_count" class="w-50" type="text">
            </div>
            <button class="m-auto btn btn-success w-50 fs-3" @click="newBook" :disabled="saving" v-if="!add_new">Hozzáadás</button>
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
                page_count: null
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
            try {
                var images = require.context('../assets/szerzok/', false, /\.jpg$/)
                return images('./' + pet + ".jpg")
            } catch (error) {
            }
        },
    
        async newBook() {
            this.saving = true
            await axios
                .post('http://127.0.0.1:8000/api/books', this.book)
                .catch(error => console.log(error))
            await this.loadData()
            
            this.saving = false
            this.resetForm()
        },

        resetForm() {
            this.book = {
                title: "",
                author: "",
                publish_year: null,
                page_count: null
            },
            this.add_new = false
        }
    },
    
    mounted() {
        this.loadData()
    }
}
</script>

<style>

</style>