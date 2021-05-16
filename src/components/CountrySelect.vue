<template>
  <select
    @change="onChange()"
    v-model="selected"
    class="form-select mt-10 block w-full border rounded p-3"
  >
    <option value="0">Selecione o País</option>
    <option v-for="country in countries" :key="country.ID" :value="country.ID">
      {{ country.Country }}
    </option>
  </select>
</template>

<script>
export default {
  name: 'CountrySelect',
  props: ['countries'],
  data() {
    return {
      selected: 0,
    }
  },
  methods: {
    onChange: function () {
      const country = this.countries.find((item) => item.ID === this.selected)
      this.$emit('get-country', country)
    },
  },
}
</script>
