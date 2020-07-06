<template>
  <v-app>
    <v-container>
      <v-row  align="center">
        <v-col>
          <h1>Pokedex vue.js</h1>
        </v-col>
        <v-col cols="3">
          <v-switch v-model="theme" label="Light/Dark theme" @change="changeTheme"></v-switch>
        </v-col>
      </v-row>
      <div v-if="error">
        error
      </div>
      <v-row v-if="!isLoading" align="center">
        <v-col sm="12" md="3">
          <v-text-field v-model="search" label="Search" @change="() => updateList(true)"></v-text-field>
        </v-col>
        <v-col sm="12" md="6">
          <v-pagination
            v-model="page"
            :length="Math.ceil(listLength / limit)"
            :total-visible="limit"
            @input="() => updateList()">
          </v-pagination>
        </v-col>
        <v-col sm="12" md="3">
          <v-select :items="pageLimits" label="Items per page" v-model="limit" @change="() => updateList(true)"></v-select>
        </v-col>
        <v-expansion-panels style="padding: 0 12px;">
          <v-expansion-panel>
            <v-expansion-panel-header>Types Filter</v-expansion-panel-header>
            <v-expansion-panel-content>
              <v-radio-group v-model="selectedType" :mandatory="false" row @change="() => updateList(true)">
                <v-radio v-for="type in typesFilter" :label="type" :value="type" v-bind:key="type" style="width: 200px; margin: 10px 0;"></v-radio>
              </v-radio-group>
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
      </v-row>
      <v-row v-if="!error">
        <div v-if="isLoading">
          <v-progress-circular :size="70" indeterminate color="primary"></v-progress-circular>
        </div>
        <v-col v-else cols="12" xs="12" sm="4" md="3" v-for="item in list" v-bind:key="item.name">
          <PokeCard :url="selectedType ==='all'?item.url:item.pokemon.url"/>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
import axios from 'axios'
import PokeCard from './components/PokeCard'
export default {
  name: 'App',

  components: {
    PokeCard
  },

  data: () => ({
    allData: null,
    list: null,
    isLoading: true,
    error: false,
    limit: 10,
    page:1,
    listLength: 1000,
    theme: true,
    pageLimits: [10, 20, 50],
    typesFilter : ["all", "bug", "dragon", "fairy", "fire", "ghost", "ground", "normal",
                  "psychic", "steel", "dark", "electric", "fighting", "flying",
                  "grass", "ice", "poison", "rock", "water"],
    selectedType: "all",
    search: ""
  }),
  mounted() {
    this.changeTheme(this.theme)
    axios
      .get('https://pokeapi.co/api/v2/pokemon/?limit=1000&offset=0')
      .then(response => {
        this.allData = response.data.results
        this.listLength = this.allData.length
        this.list = this.allData.slice(this.limit * (this.page - 1), this.limit * this.page)
      })
      .catch(error => {
        console.log(error)
        this.error = true
      })
      .finally(() => (this.isLoading = false))
  },
  methods: {
    updateList(firstPage = false) {
      let list = this.allData
      if (firstPage) {
        console.log("first")
        this.page = 1
      }
      if (this.selectedType !== "all") {
        this.isLoading = true
        axios
          .get(`https://pokeapi.co/api/v2/type/${this.selectedType}`)
          .then(response => {
            list = response.data.pokemon
            this.searchAndSlice(list)
          })
          .finally(() => (this.isLoading = false))
      } else this.searchAndSlice(list)
        
    },
    searchAndSlice(list) {
      if (this.search !== "")
        list = list.filter((item) => {
          return this.selectedType === "all"?item.name.includes(this.search):item.pokemon.name.includes(this.search)
        })
      this.listLength = list.length
      this.list = list.slice(this.limit * (this.page - 1), this.limit * this.page)

    },
    changeTheme(toggle) {
      this.$vuetify.theme.dark = toggle;
    }
  }
};
</script>