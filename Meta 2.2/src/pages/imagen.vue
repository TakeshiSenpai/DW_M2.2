<template>
    <v-container>
        <v-card>
            <v-img :src="imageUrl" height="400px"></v-img>
            <v-card-title>Chiste del Día</v-card-title>
            <v-card-text>{{ joke }}</v-card-text>
            <v-card-actions>
                <v-btn color="primary" @click="fetchData">Obtener Nueva Imagen y Chiste</v-btn>
            </v-card-actions>
        </v-card>
    </v-container>
</template>

<script>
import axios from 'axios';
export default {
    data() {
        return {
            imageUrl: '',
            joke: '',
        };
    },
    mounted() {
        this.fetchData();
    },
    methods: {
        async fetchData() {
            await this.fetchImage();
            await this.fetchJoke();
        },
        async fetchImage() {
            const random = Math.floor(Math.random() * 1000);
            this.imageUrl = `https://picsum.photos/500/300?random=${random}`;
        },
        async fetchJoke() {
            try {
                const response = await axios.get('https://api.chucknorris.io/jokes/random');
                this.joke = response.data.value;
            } catch (error) {
                console.error('Error fetching joke:', error);
                this.joke = 'Error al cargar el chiste.';
            }
        },
    },
};
</script>
