<template>
  <main v-if="!loading" class="mb-20">
    <DataTitle :text="title" :date="date" />
    <DataBoxes :stats="stats" />
    <CountrySelect @get-country="getCountryData" :countries="countries" />
    <button
      @click="clear()"
      v-if="stats.Country"
      class="
        bg-green-700
        text-white
        rounded
        p-3
        mt-10
        foucus:outline-none
        hover:bg-green-600
        text-white
        rounded
      "
    >
      Limpar seleção
    </button>
  </main>

  <main clas="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">Loading...</div>
    <div><RotateSquare2 /></div>
  </main>
</template>

<script>
import { format } from 'date-fns'
import { RotateSquare2 } from 'vue-loading-spinner'
import DataTitle from '@/components/DataTitle'
import DataBoxes from '@/components/DataBoxes'
import CountrySelect from '@/components/CountrySelect'

export default {
  name: 'Home',
  components: {
    RotateSquare2,
    DataTitle,
    DataBoxes,
    CountrySelect,
  },
  data() {
    return {
      loading: true,
      title: 'Global',
      date: '',
      stats: {},
      countries: [],
    }
  },

  methods: {
    fecthData: async function () {
      const res = await fetch('https://api.covid19api.com/summary')
      const data = await res.json()
      return data
    },

    getCountryData: function (country) {
      this.stats = country
      this.title = country.Country
    },

    clear: async function () {
      this.loading = true
      const data = await this.fecthData()
      this.title = 'Global'
      this.stats = data.Global
      this.loading = false
    },
  },

  async created() {
    const data = await this.fecthData()
    console.log(data)
    this.date = format(new Date(data.Date), 'dd/MM/yyyy - HH:mm')
    this.stats = data.Global
    this.countries = data.Countries
    this.loading = false
  },
}
</script>
