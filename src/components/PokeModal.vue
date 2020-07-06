<template>
  <div class="text-center">
    <v-dialog v-model="dialog" width="600" @click:outside="modalToggle">
      <v-card>
        <v-card-title primary-title>
            {{data.name}} #{{data.id}}
        </v-card-title>
        <v-card-text>
            <v-row>
                <v-col cols="4">
                    <img :src="data.sprites.front_default" style="image-rendering: pixelated; width: 100%"/>
                </v-col>
                <v-col cols="8">
                    <v-simple-table>
                        <template v-slot:default>
                            <tbody>
                                <tr>
                                    <td>Height</td>
                                    <td>{{data.height}}</td>
                                </tr>
                                <tr>
                                    <td>Weight</td>
                                    <td>{{data.weight}}</td>
                                </tr>
                                <tr>
                                    <td>Abilities</td>
                                    <td>{{data.abilities.map(item => " " + item.ability.name).join()}}</td>
                                </tr>
                                <tr>
                                    <td>Types</td>
                                    <td><PokeType v-for="item in data.types" v-bind:key="item.type.name" :type="item.type.name"/></td>
                                </tr>
                            </tbody>
                        </template>
                    </v-simple-table>
                </v-col>
            </v-row>
            <v-row>
                <v-col cols="3">HP</v-col>
                <v-col cols="9">
                    <v-progress-linear height="25" :value="data.stats[0].base_stat/2" striped>
                        <strong>{{data.stats[0].base_stat}}</strong>
                    </v-progress-linear>
                </v-col>
                <v-col cols="3">Attack</v-col>
                <v-col cols="9">
                    <v-progress-linear height="25" :value="data.stats[1].base_stat/2" striped>
                        <strong>{{data.stats[1].base_stat}}</strong>
                    </v-progress-linear>
                </v-col>
                <v-col cols="3">Defense</v-col>
                <v-col cols="9">
                    <v-progress-linear height="25" :value="data.stats[2].base_stat/2" striped>
                        <strong>{{data.stats[2].base_stat}}</strong>
                    </v-progress-linear>
                </v-col>
                <v-col cols="3">Special Attack</v-col>
                <v-col cols="9">
                    <v-progress-linear height="25" :value="data.stats[3].base_stat/2" striped>
                        <strong>{{data.stats[3].base_stat}}</strong>
                    </v-progress-linear>
                </v-col>
                <v-col cols="3">Special Defense</v-col>
                <v-col cols="9">
                    <v-progress-linear height="25" :value="data.stats[4].base_stat/2" striped>
                        <strong>{{data.stats[4].base_stat}}</strong>
                    </v-progress-linear>
                </v-col>
                <v-col cols="3">Speed</v-col>
                <v-col cols="9">
                    <v-progress-linear height="25" :value="data.stats[5].base_stat/2" striped>
                        <strong>{{data.stats[5].base_stat}}</strong>
                    </v-progress-linear>
                </v-col>
            </v-row>
            <PokeEvo :url="data.species.url"></PokeEvo>
        </v-card-text>
        <v-divider></v-divider>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text  @click="modalToggle">
            Close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import PokeType from './PokeType'
import PokeEvo from './PokeEvo'

export default {
    name: 'PokeModal',
    components: {
        PokeType,
        PokeEvo
    },
    props: ["data", "modalToggle"],
    data: () => ({
        dialog: true
    })
}
</script>