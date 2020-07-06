<template>
    <div v-if="isLoading" >
        <v-progress-circular :size="70" indeterminate color="primary"></v-progress-circular>
    </div>
    <v-row v-else>
        <v-col cols="4" v-for="item in data" v-bind:key="item.id">
             <img :src="item" style="image-rendering: pixelated; width: 100%"/>
        </v-col>
    </v-row>
</template>

<script>
import axios from 'axios'

export default {
    name: 'PokeEvo',
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
                axios.get(response.data.evolution_chain.url)
                .then(response => {
                    let link = "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/";
                    let evolutionSprites = [], evo = response.data.chain;
                    while (evo) {
                        let id = evo.species.url.slice(42);
                        evolutionSprites.push(link + id.substring(0, id.length - 1) + ".png")
                        evo = evo.evolves_to[0];
                    }
                    this.data = evolutionSprites;
                    console.log(this.data)
                }).finally(() => (this.isLoading = false));
            });
    },
}

</script>