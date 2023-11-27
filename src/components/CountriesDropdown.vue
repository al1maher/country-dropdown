<template>
  <!--Getting the data object and looping through it to display them-->
  <!--Note that I made the option value is the country code so that I could use it in the API-->
  <div>
    <select :value="selected" @change="handleSelect" class="flex flex-row justify-between w-48 px-2 py-2 text-gray-700 bg-white border-2 border-white rounded-md shadow focus:outline-none focus:border-blue-600" aria-label="Select a country"  placeholder="Select a Country"  :class="{ 'border-blue-600': selected }">
      <option value="" selected disabled>Select a Country</option>
      <option v-for="(country, key) in countries" :key="key" :value="key">
        <template v-if="Array.isArray(country)">
          {{ country[0] }}
        </template>
        <template v-else>
          {{ country }}
        </template>
      </option>
    </select>
  </div>
</template>

<script setup>
import { countries } from 'i18n-iso-countries/langs/en.json'

//Passing the selected country value as a state from the child component to
const { selected } = defineProps(['selected'])
const emit = defineEmits()

//Getting the selected country and emiting it as a custom event
const handleSelect = function (event) {
  const selectedValue = event.target.value
  emit('update:selected', selectedValue)
}
</script>
