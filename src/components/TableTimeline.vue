<template>
  <v-container style="overflow: scroll;" fluid>
    <v-row v-for="(item, index) in blocks" :key="item.block" style="flex-wrap: nowrap;">

      <div v-for="header in dates" :key="item.block + header.value"
        style="width: 300px; display: flex;height:400px;flex-wrap: nowrap;" class="mx-1 my-1">
        <div style="display: flex;flex-direction: column;" :style=" header.text == 'Parcela'? 'width: 100px;':'width: 300px;'">
          <p style="align-self: center;" v-if="index == 0"><strong>{{ header.text }}</strong></p>
          <p style="align-self: center;height: 400px" v-if="header.text == 'Parcela'">{{ item[header.value] }}</p>

          <v-carousel v-else-if="item[header.value] && item[header.value].length > 0">
            <v-carousel-item v-for="(photo, i) in item[header.value]" :key="i" reverse-transition="fade-transition"
              transition="fade-transition">
              <v-img height="auto" :src="backendUrl + '/images/2022-' + header.text + '/' + item.block + '/' + photo"></v-img>
            </v-carousel-item>
          </v-carousel>
          <div style="" v-else></div>
        </div>
      </div>

    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios'
export default {
  name: 'TableTimeline',
  props: {
    dates: Array,
    blocks: Array
  },
  data: () => ({
    images: [],
    backendUrl: 'http://localhost:5001'
  }),
  updated() {
    this.listBlocks();
  },
  methods: {
    listBlocks() {
      this.blocks.map((block) => {
        for (let header of this.dates) {
          if (header.text != 'Parcela') {
            axios.get(this.backendUrl + '/files/', { params: { date: header.text, block: block.block } })
              .then(response => {
                block[header.value] = response.data
              })
              .catch(error => {
                console.log(error)
              })
          }
        }
        return block
      })
    },
  }
}
</script>
