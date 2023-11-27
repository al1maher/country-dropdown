<template>
  <!--Using the Countries Dropdown List Component-->
  <div class="flex justify-center items-center h-screen bg-gray-100 flex-col">
    <div class="bg-gray-100">
      <form
        @submit.prevent="fetchAPI"
        class="bg-white p-8 rounded shadow-md flex flex-col items-center space-y-4"
      >
        <h1 class="text-3xl font-semibold mb-4 text-center">Select Your Favorite Country</h1>
        
        <!--Accepting the change event and the selected data from the child component-->
        <CountriesDropdown :selected="selected" @update:selected="updateSelected" />
        <button
          type="submit"
          class="bg-blue-500 text-white py-2 px-4 rounded hover:bg-blue-600 focus:outline-none focus:ring focus:border-blue-300"
        >
          Submit
        </button>
      </form>
    </div>

    <!--Making A Smooth Transition for the Data-->
    <transition name="scale">

      <!--A Placeholder that dispays untill the API response is back-->
      <div v-if="showLoading" class="mt-8">
        <div class="text-center text-gray-500">Loading...</div>
      </div>

      <!--Displaying The Selected country info using an API-->
      <div v-else-if="countryInfo.name" class="mt-8">
        <h2 class="text-4xl font-semibold mb-4 text-center">
          Your Selected Country is {{ countryInfo.name }}
        </h2>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
          <div class="bg-white p-4 rounded shadow-md">
            <p><span class="font-bold">Capital:</span> {{ countryInfo.capital }}</p>
            <p><span class="font-bold">Country Code:</span> {{ countryInfo.code }}</p>
            <p><span class="font-bold">Calling Code:</span> {{ countryInfo.callingCode }}</p>
            <p><span class="font-bold">Currency:</span> {{ countryInfo.currencyCodes[0] }}</p>
            <p><span class="font-bold">Number of Regions:</span> {{ countryInfo.numRegions }}</p>
          </div>
          <div class="bg-white p-4 rounded shadow-md col-span-2 flex items-center justify-center">
            <p class="text-center"><span class="font-bold">Flag:</span></p>
            <img
              class="rounded-full h-32 w-32 mx-auto mb-4"
              :src="countryInfo.flagImageUri"
              :alt="countryInfo.name"
            />
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script setup>
import CountriesDropdown from './components/CountriesDropdown.vue'
import { ref, computed } from 'vue'
import axios from 'axios'

//Making reactive state
let selected = ref('')
let countryInfo = ref({})

let showLoading = ref(false)

//Reciving the selected country value and storing it as a state.
const updateSelected = function (value) {
  selected.value = value
}

//The data of the API, in a computed property to watch the changes and reflect them reactively.
const options = computed(() => {
  return {
    method: 'GET',
    url: `https://wft-geo-db.p.rapidapi.com/v1/geo/countries/${selected.value}`,
    headers: {
      'X-RapidAPI-Key': '7229adc070msh26d4e2c32a6cd6cp13204ejsn1e0f423a3faa',
      'X-RapidAPI-Host': 'wft-geo-db.p.rapidapi.com'
    }
  }
})

//Using Axios and Async Await API to fetch the data from the API
async function fetchAPI() {
  showLoading.value = true
  countryInfo.value = {}

  try {
    const response = await axios.request(options.value)
    showLoading.value = false
    countryInfo.value = response.data.data
  } catch (error) {
    console.error(error)
  }
}
</script>

<!---Local scoped styling for the transition -->
<style scoped>
.scale-enter-active,
.scale-leave-active {
  transition: transform 0.5s cubic-bezier(0.165, 0.84, 0.44, 1);
}

.scale-enter-from,
.scale-leave-to {
  transform: scale(0.5);
}
</style>
