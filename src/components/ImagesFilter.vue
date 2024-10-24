<template>
  <div>
    <v-container>
      <v-row class="text-center">
        <v-col cols="12">
          <h3><strong>Filtros</strong></h3>
        </v-col>
        <v-col>
          <v-select ref="selector-gender" v-model.lazy="genderFilter" :items="gendersAvailable" v-click-outside="{handler: changeAvailability}" attach chips label="Gênero"
          multiple></v-select>
        </v-col>
        <v-col>
          <v-select ref="selector-specie" v-model.lazy="specieFilter" :items="speciesAvailable" v-click-outside="{handler: changeAvailability}" attach chips label="Espécie"
          multiple></v-select>
        </v-col>
        <v-col>
          <v-select ref="selector-cultivar" v-model.lazy="cultivarFilter" :items="cultivarAvailable" v-click-outside="{handler: changeAvailability}"  attach chips label="Cultivar"
          multiple></v-select>
        </v-col>
        <v-col cols="12" class="my-2 pt-0">
          <v-btn  @click="clearFilters()" color="primary">Limpar Filtros</v-btn>
        </v-col>
      </v-row>
      <v-divider></v-divider>
      <v-row class="text-center">
        <v-col cols="12">
          <h3><strong>Seletores</strong></h3>
        </v-col>
        <v-col cols="6">
          <v-select v-model="selectedHeaders" :items="headers"  item-text="text" return-object attach chips label="Datas"
          multiple></v-select>
        </v-col>
        <v-col cols="6">
          <v-select v-model="selectedBlocks" :items="availableBlocks" item-text="block" return-object attach chips label="Parcelas" multiple>
            <template slot='item' slot-scope='{ item }' >
              <span>{{ item.block +' - '+ item.gender +' '+ item.species +' '+ item.cultivar}}</span>
            </template>
          </v-select>
        </v-col>
      </v-row>
    </v-container>
    <v-container fluid>
      <v-divider></v-divider>
      <table-timeline :key="tableKey" :blocks="selectedBlocks" :dates="selectedHeaders"></table-timeline>
    </v-container>
  </div>
</template>

<script>
import blocks from '../assets/blocks.json'
import TableTimeline from './TableTimeline.vue';

export default {
  name: 'HelloWorld',
  components: {
    TableTimeline
  },
  data: () => ({
    headers: [{ text: 'Parcela', value: 'block' }, { text: '02-25', value: '02-25' }, { text: '03-05', value: '03-05' }, { text: '03-11', value: '03-11' }, { text: '03-18', value: '03-18' }, { text: '03-29', value: '03-29' }, { text: '04-09', value: '04-09' },
    { text: '04-23', value: '04-23' }, { text: '04-28', value: '04-28' }, { text: '05-21', value: '05-21' }, { text: '07-15', value: '07-15' }, { text: '07-22', value: '07-22' }, { text: '08-06', value: '08-06' },
    { text: '08-12', value: '08-12' }, { text: '08-20', value: '08-20' }, { text: '09-09', value: '09-09' }, { text: '09-16', value: '09-16' }, { text: '09-23', value: '09-23' }, { text: '10-29', value: '10-29' }],
    blocks: blocks,
    selectedHeaders: [],
    selectedBlocks: [],
    tableKey: 0,
    dir: "/Volumes/mac 1/Forrageiras_",
    images: [],
    availableBlocks: blocks,
    genderFilter: [],
    specieFilter: [],
    cultivarFilter: [],
    gendersAvailable: [],
    cultivarAvailable: [],
    speciesAvailable: []
  }),
  mounted: function () {
    this.changeAvailability();
  },
  methods: {
    changeAvailability() {
      if (!this.$refs['selector-cultivar'].$data.isFocused){
        this.availableBlocks = this.availableBlocks.filter((el) => {
        if (
        ((this.genderFilter.length == 0 || (this.genderFilter != [] && this.genderFilter.includes(el.gender))) && 
        (this.specieFilter.length == 0 || (this.specieFilter != [] && this.specieFilter.includes(el.species))) && 
        (this.cultivarFilter.length == 0 || (this.cultivarFilter != [] && this.cultivarFilter.includes(el.cultivar))))) {
          return el
        }
      })
      this.loadFilters();
      }
      
    },
    loadFilters() {
      this.gendersAvailable = []
      this.speciesAvailable = []
      this.cultivarAvailable = []
      this.availableBlocks.forEach((item)=>{
        if(!this.gendersAvailable.includes(item.gender)) {
          this.gendersAvailable.push(item.gender)
        }
        if(!this.speciesAvailable.includes(item.species)) {
          this.speciesAvailable.push(item.species)
        }
        if(!this.cultivarAvailable.includes(item.cultivar)) {
          this.cultivarAvailable.push(item.cultivar)
        }
      })
    },
    clearFilters (){
      this.genderFilter = []
      this.specieFilter = []
      this.cultivarFilter = []
      this.availableBlocks = this.blocks
      this.loadFilters();
    }
  }
}
</script>
