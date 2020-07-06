<template>
    <div>
        <div v-if="isLoading">
            <v-progress-circular :size="70" indeterminate color="primary"></v-progress-circular>
        </div>
        <v-card v-if="!isLoading">
            <v-img :src="data.sprites.front_default" style="image-rendering: pixelated"/>
            <v-card-title>{{data.name}} #{{data.id}}</v-card-title>
            <v-card-subtitle>
                <PokeType v-for="item in data.types" v-bind:key="item.type.name" :type="item.type.name"/>
            </v-card-subtitle>
            <v-card-actions>
                <v-btn color="primary" text style="width: 100%" @click="showModal">
                    Show more info
                </v-btn>
            </v-card-actions>
            <PokeModal :data="data" v-if="modal" :modalToggle="showModal"/>
        </v-card>
    </div>
</template>

<script>
import axios from 'axios'
import PokeType from './PokeType'
import PokeModal from './PokeModal'

export default {
    name: 'PokeCard',

    components: {
        PokeType,
        PokeModal
    },
    props: ["url"],
    data: () => ({
        data: null,
        isLoading: true,
        error: false,
        modal: false
    }),
    mounted() {
        axios
            .get(this.url)
            .then(response => {
                this.data = response.data;
            })
            .catch(error => {
                console.log(error);
                this.error = true;
            })
            .finally(() => (this.isLoading = false));
    },
    methods: {
        showModal() {
            this.modal = !this.modal
        }
    }

};
</script>
