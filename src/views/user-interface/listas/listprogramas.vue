<template>
  <VCard>
    <VCardText class="d-flex flex-column gap-y-8" style="background-color:#E6E6E6;">
      <v-select
        v-model="programa"
        :items="items"
        item-value="id"
        item-title="nombre"
        label="Selecciona un programa"
        return-object
      ></v-select>
    </VCardText>
  </VCard>
</template>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      items: [],
      programa: null,
    }
  },

  methods: {},
  async mounted() {
    const response = await axios.get('http://localhost:3000/programa/', {
      headers: {
        Authorization: `Bearer ${this.$store.getters.getUser.access_token}`,
      },
    })
    this.items = response.data
    console.log(this.items)
  },

  watch: {
    programa() {
      // let arreglo2 = this.items
      //let obj = arreglo2.find(objeto => objeto.id =  this.programa);
      this.$emit('selprograma', this.programa)
    },
  },
}
</script>
